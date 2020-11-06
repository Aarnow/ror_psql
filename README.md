# How to use PostgreSQL with Ruby on Rails API ?
- Initialize your Ruby on rails API
- Connect your database with this API
- Generate your first model
- Generate your first controller
- Write your first route

### Initialize your Ruby on rails API

**--api:** we are doing an API
**--no-sprockets:** disable <a href="https://github.com/rails/sprockets">sprokects library</a>
**-d postgresql:** announce that our database will be posgresql

```shell
rails new project_name --api --no-sprockets -d postgresql
```

### Connect your database with this API

```shell
psql #enter psql command
ALTER USER my_name with encrypt password 'my_password' #create your user
CREATE DATABASE db_name; #create your database
```

```ruby
#config/database.yml
development:
  adapter: postgresql
  encoding: unicode
  database: db_name
  pool: 5
  username: my_name
  password: my_password
  port: 5432 #(default: 5432)
```

### Generate your first model (table)

```shell
rails g model contacts #create your table/model "contact"
```

 Files are created. to add columns to the "contact" table, go to ***db/migrate/date_code***

 ```ruby
 class CreateContacts < ActiveRecord::Migration[6.0]
  def change
    create_table :contacts do |t|
      #type and name column:
      t.string :first_name
      t.string :last_name
      t.string :email
      
      t.timestamps
    end
  end
end
 ```

Push in your databse:

 ```shell
 rails db:migrate
 
 #result:
 == 20201105081314 CreateContacts: migrating ===================================
-- create_table(:contacts)
   -> 0.0162s
== 20201105081314 CreateContacts: migrated (0.0162s) ==========================
 ```

### Generate your first controller

```shell
rails g controller v1/contacts
```

generate:

```ruby
#controllers/v1/contacts_controller.rb

class V1::ContactsController < ApplicationController

  #This method for return all contacts 
  def index
    @contacts = Contact.all

    render json: @contacts, status: :ok
  end

end
```

### Write your first route

```ruby
#config/routes.rb
Rails.application.routes.draw do

  namespace :v1 do
    resources :contacts
  end

end
```

check your available routes:

```shell
rails routes
```
