# jsonerl

It is modified version on Bob Ippolito's <bob@mochimedia.com> mochijson2 module, one can find in excellent mochiweb, erlang webserver.

Yet another JSON (RFC 4627) library for Erlang. jsonerl works with binaries as strings, arrays as lists (without an {array, _}) wrapper and it only knows how to decode UTF-8 (and ASCII).

## Mapping

| Erlang data type | JavaScript data type | Erlang data type      |
|------------------|----------------------|-----------------------|
| atom true        | boolean true         | atom true             |
| atom false       | boolean false        | atom false            |
| atom undefined   | null                 | atom,undefined        |
| any other atom   | string               | binary holding string |
| list             | array                | list                  |
| number           | number               | number                |
| binary           | string               | binary                |
| proptuple        | object               | proptuple             |

*Note:* proptuple is result of passing proplist to list_to_atom bif ;)
This is concept of mine to nicely model javascript object without a need of tagging structures
on erlang side.

## Example

The following JavaScript object

```js
{
     name: "Luc Besson",
     year_of_birth: 1959,
     city: "Paris",
     photo: "http://is.gd/3pYJF",
     movies: ["Taken","Bandidas","Taxi"]
}
```

is represented like this:

```erlang
{
  {<<"name">>, <<"Luc Besson">>},
  {<<"year_of_birth">>, 1959},
  {<<"city">>, <<"Paris">>},
  {<<"photo">>, <<"http://is.gd/3pYJF">>},
  {<<"movies">>, [<<"Taken">>,<<"Bandidas">>,<<"Taxi">>]}
}
```

## Utils

Additionally there are utility macros delivered with jsonerl that helps turning erlang records to
json and back.

### Example of a single item:

```erlang
-include("jsonerl.hrl").
-record(artist, {name, year_of_birth, city, photo, movies}).

Artist = #artist{
  name = <<"Luc Besson">>,
  year_of_birth = 1959,
  city = <<"Paris">>,
  photo = <<"http://is.gd/3pYJF">>,
  movies = [<<"Taken">>,<<"Bandidas">>,<<"Taxi">>]
},
Json = ?record_to_json(artist, Artist),
Artist = ?json_to_record(artist, Json).
```

The resulting json will be:

```js
{
     name: "Luc Besson",
     year_of_birth: 1959,
     city: "Paris",
     photo: "http://is.gd/3pYJF",
     movies: ["Taken","Bandidas","Taxi"]
}
```

### Example of a list:

```erlang
-record(man,{name,age}).

Man1 = #man{name = "Petr", age = 45},
Man2 = #man{name = "Elton", age = 27 },
ListOfMans = [Man1,Man2],
ListJson = ?list_record_to_json(man,ListOfMans)
```

The resulting json will be:

```js
[
  {
    name:"Petr",
    age:45
  },
  {
    name:"Elton",
    age:27
  }
]
```
