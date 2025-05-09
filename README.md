# sqlc-gen-better-python
A WASM plugin for SQLC allowing the generation of Python code.


> [!NOTE]  
> This is currently being worked on. It is far from being ready for any kind of release, let alone a stable one.  
> Please wait for the v1 release; before that, this plugin is likely to not work.

## Configuration Options
| Name                             | Type         | Required | Description                                                                                                                                                                                                               |
|----------------------------------|--------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `package`                        | string       | yes      | The name of the package where the generated files will be located                                                                                                                                                         |
| `emit_init_file`                 | bool         | yes      | If set to to `false` there will be no `__init__.py` file created in the package that you specified. Only set this to false if you know that you already have a `__init__.py` file otherwise the generated code wont work. |
| `sql_driver`                     | string       | no       | The name of the sql driver you want to use. Defaults to `aiosqlite`. Valid options are listed [here](#feature-support)                                                                                                    |
| `model_type`                     | string       | no       | The model type you want to use. This can be one of `dataclass`, `msgspec` or `attrs`. Defaults to `dataclass`                                                                                                             |
| `initialisms`                    | list[string] | no       | An array of [initialisms](https://google.github.io/styleguide/go/decisions.html#initialisms) to upper-case. For example, `app_id` becomes `AppID`. Defaults to `["id"]`.                                                  |
| `emit_exact_table_names`         | bool         | no       | If `true`, model names will mirror table names. Otherwise sqlc attempts to singularize plural table names.                                                                                                                |
| `emit_classes`                   | bool         | no       | If `true`, every query function will be put into a class called `Querier`. Otherwise every function will be a standalone function.                                                                                        |
| `inflection_exclude_table_names` | list[string] | no       | An array of table names that should not be turned singular. Only applies if `emit_exact_table_names` is `false`.                                                                                                          |
| `omit_unused_models`             | bool         | no       | If set to `true` and there are models/tables that are not used in any query, they wont be turned into models.                                                                                                             |
| `query_parameter_limit`          | integer      | no       | Not yet implemented.                                                                                                                                                                                                      |
| `debug`                          | bool         | no       | If set to `true`, there will be debug logs generated into a `log.txt` file when executing `sqlc generate`. Defaults to `false`                                                                                            |

## Feature Support
Every [sqlc macro](https://docs.sqlc.dev/en/latest/reference/macros.html) is supported.
The supported [query commands](https://docs.sqlc.dev/en/latest/reference/query-annotations.html) depend on the SQL driver you are using, supported commands are listed below.
> Every `:batch*` command is not supported by this plugin and probably will never be.

> Prepared Queries are not planned for the near future, but will be implemented sooner or later

> [!NOTE]  
> Asyncpg only has very bad support until now. It doesn't support `:execresult`, `:execrows` and `:execlastid`

|           | `:exec` | `:execresult` | `:execrows` | `:execlastid` | `:many` | `:one` | `:copyfrom` |
|-----------|---------|---------------|-------------|---------------|---------|--------|-------------|
| aiosqlite | yes     | yes           | yes         | yes           | yes     | yes    | no          |
| sqlite3   | yes     | yes           | yes         | yes           | yes     | yes    | no          |
| asyncpg   | yes     | no            | no          | no            | yes     | yes    | no          |
| psycopg2  | no      | no            | no          | no            | no      | no     | no          |
| mysql     | no      | no            | no          | no            | no      | no     | no          |

## Development
A roadmap of what is planned & worked on can be found [here](https://github.com/users/rayakame/projects/1/)
### Changelog
Can be found [here](https://github.com/rayakame/sqlc-gen-better-python/blob/main/CHANGELOG.md)

## Credits
Because of missing documentation about creating these plugins, this work is heavily 
inspired by:
- [sqlc-gen-go](https://github.com/sqlc-dev/sqlc-gen-go)
- [sqlc-gen-java](https://github.com/tandemdude/sqlc-gen-java)

Special thanks to [tandemdude](https://github.com/tandemdude) for answering my questions on discord.