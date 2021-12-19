
"You can get everything in life you want if you will just help enough other people get what they want —Zig Ziglar"

=============================
Laravel Datatable Tutorial
=============================
0) Server Requirement 
   PHP version PHP 7.3+ or above

1) Install Laravel using following command.
   composer create-project laravel/laravel datatable

2) Run project using following command in your project folder
   php artisan serve   

3) Create a Database called "datatable_db" and table called "users", sql is attached in project description 
   and you can use it. You also have to configure your database using .env file.

4) Make a controller called "UserController" using following command
   php artisan make:controller UserController

5) Create a method called "index" in "UserController"   

6) Now Create a view called "list.blade.php"
   Location: resources\views\list.blade.php

7) Create a get route in web.php

8) Let's load "list" view in "index" method.

9) Now add datatable js and css.

10) Let write js code for datatable initialization in list view. 

   $(document).ready(function(){
      $('#datatable').DataTable({
         processing: true,
         serverSide: true,
         order: [[ 0, "desc" ]],
         ajax: "{{ url('users-data') }}",
         columns: [
               { data: 'id' },
               { data: 'name' },
               { data: 'email' },
               { data: 'created_at' },
               { data: 'updated_at' }        
         ]
      });
   });

10) Now we will create a "getData" method which will return required json data for datatable.

   $draw 				= 		$request->get('draw'); // Internal use
   $start 				= 		$request->get("start"); // where to start next records for pagination
   $rowPerPage 		= 		$request->get("length"); // How many recods needed per page for pagination

   $orderArray 	   = 		$request->get('order');
   $columnNameArray 	= 		$request->get('columns'); // It will give us columns array
                     
   $searchArray 		= 		$request->get('search');
   $columnIndex 		= 		$orderArray[0]['column'];  // This will let us know,
                                                      // which column index should be sorted 
                                                      // 0 = id, 1 = name, 2 = email , 3 = created_at

   $columnName 		= 		$columnNameArray[$columnIndex]['data']; // Here we will get column name, 
                                                                  // Base on the index we get

   $columnSortOrder 	= 		$orderArray[0]['dir']; // This will get us order direction(ASC/DESC)
   $searchValue 		= 		$searchArray['value']; // This is search value 

   $response = array(
      "draw" => intval($draw),
      "recordsTotal" => $total,
      "recordsFiltered" => $totalFilter,
      "data" => $arrData,
   );