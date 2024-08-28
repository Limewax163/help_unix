# PHP Artisan (Laravel)

- `php artisan migrate:status` - посмотреть статус всех миграций
- `php artisan migrate:rollback` - откатывает все выполненные миграции
  - `--step=<number>` - откатывает выбранное количество групп миграций (batch group number)
  - `--path=<path/to/migrate_file>` - отменяет выборочно миграцию (p.s. пока работает криво, не разобрался)
- `php artisan migrate:refresh` - делает для всех миграций rollback а затем migrate

>[!NOTE]
>Если при выполнении миграций есть ошибки что миграции уже существуют "already exist", то есть два варианта решения:
>1. Указать в самой БД статус миграции
>2. Дропнуть таблицу и выполнить миграцию

<details>
  <summary><b>Возможные ошибки</b></summary>

  ## Ошибка при удалении таблицы связанная с наличием внешних ключей
  ```
  ERROR 1217 (23000): Cannot delete or update a parent row: a foreign key constraint fails
  ```

  <details>
    <summary>решение</summary>
  Посмотреть ключи:
    
  ```
  SELECT table_name, constraint_name 
  FROM information_schema.key_column_usage 
  WHERE referenced_table_name = 'table_name' 
  AND table_schema = 'database_name';
  ```
  
  Удалить ключи:

  ```
  ALTER TABLE <table> DROP FOREIGN KEY <key>;
  ```
  </details>

  ## При выполнении миграций ошибка что таблица уже существуют "already exist"
  Есть два варианта решения:

  <details>
    <summary>1. Обозначить миграции как завершенные в БД</summary>
    
    ```
    INSERT INTO migrations (migration, batch) VALUES
    ('<migration_title', <batch_number>),
    ('<migration_title', <batch_number>),
    ('<migration_title', <batch_number>);
    ```
  </details> 
  2. Дропнуть таблицу и выполнить миграцию
  
</details>
