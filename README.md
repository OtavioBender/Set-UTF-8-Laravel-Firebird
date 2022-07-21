# Set-UTF-8-Laravel-Firebird
Error: "Malformed UTF-8 characters, possibly incorrectly encoded", when trying to return data with base Firebird utf8 field

## Install : 
composer require harrygulliford/laravel-firebird


## Use to send to Frontend:
$data = [
  'status' => true,
  'msg' => ' ',
  'data' => $dataofreturn,
];
return $this->returnQuery($data);

## Alter function returnQuery in Controller.php:

    public function returnQuery(array $query)
    { 
        if ($query['status']) {
            return response()->json(
                $query['data'],
                200,
                ['Content-Type' => 'application/json;charset=UTF-8', 'Charset' => 'utf-8'],
                JSON_UNESCAPED_UNICODE  | JSON_UNESCAPED_SLASHES | JSON_NUMERIC_CHECK
            );
        }

        return response()->json($query, 400);
    }
    
## Alter config databese in databese.php:

'connections' => [
  'firebird' => [
        'driver'   => 'firebird',
        'host'     => env('DB_HOST', 'localhost'),
        'port'     => env('DB_PORT', '3050'),
        'database' => env('DB_DATABASE', '/path_to/database.fdb'),
        'username' => env('DB_USERNAME', 'sysdba'),
        'password' => env('DB_PASSWORD', 'masterkey'),
        'charset'  => env('DB_CHARSET', 'UTF8'),
        'collation' => 'utf8_general_ci'
        'role'     => null,
    ],
 }   

