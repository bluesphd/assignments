

<?php

/**
 * UberGallery is an easy to use, simple to manage, web photo gallery written in
 * PHP. UberGallery does not require a database and supports JPEG, GIF and PNG
 * file types. Simply upload your images and UberGallery will automatically
 * generate thumbnails and output standards complaint XHTML markup on the fly.
 *
 * This software is distributed under the MIT License
 * http://www.opensource.org/licenses/mit-license.php
 *
 * More info available at http://www.ubergallery.net
 *
 * @author Chris Kankiewicz <Chris@ChrisKankiewicz.com>
 * @copyright Copyright (c) 2013 Chris Kankiewicz (http://www.chriskankiewicz.com)
 * @license http://www.opensource.org/licenses/mit-license.php MIT License
 * @link https://github.com/UberGallery/UberGallery Cannonical source URL
 */
class UberGallery {

    // Define application version
    const VERSION = '2.4.4';

    // Reserve some variables
    protected $_config     = array();
    protected $_imgDir     = NULL;
    protected $_appDir     = NULL;
    protected $_index      = NULL;
    protected $_rThumbsDir = NULL;
    protected $_rImgDir    = NULL;
    protected $_now        = NULL;


    /**
     * UberGallery construct function. Runs on object creation.
     */
    public function __construct() {

        // Get timestamp for the current time
        $this->_now = time();

        // Sanitize input and set current page
        if (isset($_GET['page'])) {
            $this->_page = (integer) $_GET['page'];
        } else {
            $this->_page = 1;
        }

        // Set class directory constant
        if(!defined('__DIR__')) {
            define('__DIR__', dirname(__FILE__));
        }

        // Set application directory
        $this->_appDir = __DIR__;

        // Set configuration file path
        $configPath = $this->_appDir . '/galleryConfig.ini';

        // Read and apply gallery config or throw error on fail
        if (file_exists($configPath)) {
            // Parse gallery configuration
            $config = parse_ini_file($configPath, true);

            // Apply configuration
            $this->setCacheExpiration($config['basic_settings']['cache_expiration']);
            $this->setPaginatorThreshold($config['basic_settings']['paginator_threshold']);
            $this->setThumbSize($config['basic_settings']['thumbnail_width'], $config['basic_settings']['thumbnail_height']);
            $this->setThumbQuality($config['basic_settings']['thumbnail_quality']);
            $this->setThemeName($config['basic_settings']['theme_name']);
            $this->setSortMethod($config['advanced_settings']['images_sort_by'], $config['advanced_settings']['reverse_sort']);
            $this->setDebugging($config['advanced_settings']['enable_debugging']);
            $this->setCacheDirectory($this->_appDir . '/cache');

            if ($config['basic_settings']['enable_pagination']) {
                $this->setImagesPerPage($config['advanced_settings']['images_per_page']);
            } else {
                $this->setImagesPerPage(0);
            }

        } else {
            die("Unable to read galleryConfig.ini, please make sure the file exists at: <pre>{$configPath}</pre>");
        }

        // Get the relative thumbs directory path
        $this->_rThumbsDir = $this->_getRelativePath(getcwd(), $this->_config['cache_dir']);

        // Check if cache directory exists and create it if it doesn't
        if (!file_exists($this->_config['cache_dir'])) {
            $this->setSystemMessage('error', "Cache directory does not exist, please manually create it.");
        }

        // Check if cache directory is writeable and warn if it isn't
        if (!is_writable($this->_config['cache_dir'])) {
            $this->setSystemMessage('error', "Cache directory needs write permissions. If all else fails, try running: <pre>chmod 777 {$this->_config['cache_dir']}</pre>");
        }

        // Set debug log path
        $this->_debugLog = $this->_config['cache_dir'] . '/debug.log';

        // Set up debugging if enabled
        if ($this->_config['debugging']) {

            // Initialize log if it doesn't exist
            if (!file_exists($this->_debugLog)) {

                // Get libgd info
                $gd = gd_info();

                // Get system and package info
                $timestamp  = date('Y-m-d H:i:s');
                $ugVersion  = 'UberGallery v' . UberGallery::VERSION;
                $phpVersion = 'PHP: ' . phpversion();
                $gdVersion  = 'GD: ' . $gd['GD Version'];
                $osVersion  = 'OS: ' . PHP_OS;

                // Combine all the things!
                $initText = $timestamp . ' / ' . $ugVersion . ' / ' . $phpVersion . ' / ' . $gdVersion . ' / ' . $osVersion . PHP_EOL;

                // Create file with initilization text
                file_put_contents($this->_debugLog, $initText, FILE_APPEND);
            }

            // Set new error handler
            set_error_handler("UberGallery::_errorHandler");

        }

    }


    /**
     * Special init method for simple one-line interface
     *
     * @return reflection
     * @access public
     */
    public static function init() {
        $reflection = new ReflectionClass(__CLASS__);
        return $reflection->newInstanceArgs(func_get_args());
    }


    /**
     * Returns pre-formatted XHTML of a gallery
     *
     * @param string $directory Relative path to images directory
     * @param string $relText Text to use as the rel value
     * @return object Self
     * @access public
     */
    public function createGallery($directory, $relText = 'colorbox') {

        // Get the gallery data array and set the template path
        $galleryArray = $this->readImageDirectory($directory);
        $templatePath = $this->_appDir . '/templates/defaultGallery.php';

        // Set the relative text attribute
        $galleryArray['relText'] = $relText;

        // Echo the template contents
        echo $this->readTemplate($templatePath, $galleryArray);

        return $this;

    }


    /**
     * Returns an array of files and stats of the specified directory
     *
     * @param string $directory Relative path to images directory
     * @return array File listing and statistics for specified directory
     * @access public
     */
// well documented and fairly easy to follow what the code is doing