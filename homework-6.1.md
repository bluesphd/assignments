## Homework 6.1: Follow the Arrows!

* Use your upstream remote to pull homework-6.1.md into your workspace on Cloud9 (hint: git pull upstream master).
 
* Find a section in your projects that has some decent looping and branching code: at least five branches that you can diagram.
* Copy-paste your example into homework-6.1.md and attempt to identify the loop conditions with comments e.g. 

```php
while ( $count < $max ) {
// while $count is less than $max

foreach ( $collection as $item ) {
// until there are no more $items in the $collection"
```

* Save your file locally, git add and git commit it (don't forget: -m 'explain why!'), and git push your changes to your Github account.
* **Bonus points:** open a pull request back to the original repo.

if ( $wp_error->get_error_code() ) {
		$errors = '';
		$messages = '';
		foreach ( $wp_error->get_error_codes() as $code ) {
			$severity = $wp_error->get_error_data($code);
			foreach ( $wp_error->get_error_messages($code) as $error ) {
				if ( 'message' == $severity )
					$messages .= '	' . $error . "<br />\n";
				else
					$errors .= '	' . $error . "<br />\n";
			}
		}
		if ( !empty($errors) )
			echo '<div id="login_error">' . apply_filters('login_errors', $errors) . "</div>\n";
		if ( !empty($messages) )
			echo '<p class="message">' . apply_filters('login_messages', $messages) . "</p>\n";
	}
} // End of login_header()

// Function declared in ubergallery program
    private function _arrayPaginate($array, $resultsPerPage, $currentPage) {
        // Page varriables
        $totalElements = count($array);

        if ($totalElements == 0) {

            $paginatedArray = array();

        } else {

            if ($resultsPerPage <= 0 || $resultsPerPage >= $totalElements) {
                $firstElement = 0;
                $lastElement = $totalElements;
                $totalPages = 1;
            } else {
                // Calculate total pages
                $totalPages = ceil($totalElements / $resultsPerPage);

                // Set current page
                if ($currentPage < 1) {
                    $currentPage = 1;
                } elseif ($currentPage > $totalPages) {
                    $currentPage = $totalPages;
                } else {
                    $currentPage = (integer) $currentPage;
                }

                // Calculate starting image
                $firstElement = ($currentPage - 1) * $resultsPerPage;

                // Calculate last image
                if($currentPage * $resultsPerPage > $totalElements) {
                    $lastElement = $totalElements;
                } else {
                    $lastElement = $currentPage * $resultsPerPage;
                }
            }

            // Initiate counter
            $x = 1;

            // Run loop to paginate images and add them to array
            foreach ($array as $key => $element) {

                // Add image to array if within current page
                if ($x > $firstElement && $x <= $lastElement) {
                    $paginatedArray[$key] = $array[$key];
                }

                // Increment counter
                $x++;
            }

        }

        // Return paginated array
        return $paginatedArray;
    }
    
    // Code in ubergallery that calls the above function
    // Paginate the array and return current page if enabled
        if ($paginate == true && $this->_config['img_per_page'] > 0) {
            $dirArray = $this->_arrayPaginate($dirArray, $this->_config['img_per_page'], $this->_page);
    // $dirArray is 1st argument in function _arraypaginate named $array and will be used inside the fuction code
    // $this etc is 2nd argument, and $this etc is the 3rd argument