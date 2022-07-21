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

'connections' => [ <br />
  'firebird' => [ <br />
        'driver'   => 'firebird', <br />
        'host'     => env('DB_HOST', 'localhost'), <br />
        'port'     => env('DB_PORT', '3050'), <br />
        'database' => env('DB_DATABASE', '/path_to/database.fdb'), <br />
        'username' => env('DB_USERNAME', 'sysdba'), <br />
        'password' => env('DB_PASSWORD', 'masterkey'), <br />
        'charset'  => env('DB_CHARSET', 'UTF8'), <br />
        'collation' => 'utf8_general_ci' <br />
        'role'     => null, <br />
    ], <br />
 ]

