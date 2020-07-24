---
layout: post
title:      "3 things I learned when making my first Sinatra app"
date:       2020-07-24 01:59:58 +0000
permalink:  3_things_i_learned_when_making_my_first_sinatra_app
---


## 1. Don't forget about raw SQL

When seeding a database from custom information you have a few options. You can create the objects and persist the objects to the databse, you can import a csv file, and you can run raw sql statements. 

### Method 1
The first requires very little effort if you are seeding the database with 2 objects that have a belongs to and has many relationship

```
user = User.create
user.notes.build

```

### Method 2
The second method(seeding with csv) is a complicated wreck of what I deem to be unneccesary complexity at this point in time. If you have multiple models you will need to write out each models attribute(column names) as if creating a migration. 

```
require 'csv'

csv_text = File.read(Rails.root.join('lib', 'seeds', 'real_estate_transactions.csv'))
csv = CSV.parse(csv_text, :headers => true, :encoding => 'ISO-8859-1')
csv.each do |row|
  t = Transaction.new
  t.street = row['street']
  t.city = row['city']
  t.zip = row['zip']
  t.zip = row['zip']
  t.state = row['state']
  t.beds = row['beds']
  t.sq_feet = row['sq_feet']
  t.category = row['type']
  t.sale_date = row['sale_date']
  t.price = row['price']
  t.lat = row['latitude']
  t.lng = row['longitude']
  t.save
  puts "#{t.street}, #{t.city} saved"
end

puts "There are now #{Transaction.count} rows in the transactions table"
```

*(On a side note if you are using someone elses data without an API this is a great way to get their data into your database.)*

### Method 3
The method that worked the best for me was using raw SQL. My database has a total of 10 tables and 4 join tables with multiple belongs to, has manys, and a few has many through relationships.

To make this as easy as possible I used [Db Browser for SQLite](https://sqlitebrowser.org/) to export my current database as raw SQL into the seeds.rb file.

Once the data is exported open up seeds.rb and enter in the following.

```
DB = {:conn => SQLite3::Database.new("./db/development.sqlite")}

sql = <<-SQL
#wrap all your sql statements in the Heredoc.
SQL

DB[:conn].execute_batch(sql)
```

Take a closer look at the above again. In the Sinatra application we have been using our database connection that was set up in our environment/yaml file, and low and behold execute_batch isn't recognized by the ActiveRecord.base connection. Round 2 learning the power of raw SQL. If you wanted to use ActiveRecord you would need to iterate over each SQL statement individually. On line one I established the connection to my development with SQLite and gained access to the execute_batch method. 


## 2. Use a CSS frameworks like Materialize make life better.

If you are focusing on writing excellent code and trying to put out an MVP(minimal viable product) CSS frameworks will speed up making your app presentable for the masses. 

You can go from something with no custom styling to a pleasant to the eye website in a matter of moments. 

![](https://materializecss.com/images/starter-template.gif)

The peace of mind of using a Materialize allowed me to push the boundaries of what I knew and create user friendly front end with what is to me a complex back-end. 

## 3. Using a database correctly takes work, and a lot of doc reading.

When working with your database you should always be making decision based on performance. I discovered that iterating over records with ruby was already accounted for with active record, and in fact using ActiveRecord methods was reccommended over ruby iterations due to future performance issues with large databases. 

When iterating over the database with vanilla ruby you will go through each record and create an Object that can be iterated over. A huge database would start to have memory nightmares! 

```
 current_user.recipes.each do |recipe|%>
   <%if !current_user.recipes.empty?       
    current_user.recipes.each do |recipe|
      unless recipe.brew_logs.empty?
       #code block
     current_user.recipes.find_by_id(recipe.id).brew_logs.each do |log|
	     #code block
	   end
```

So instead of doing the above use the methods that ActiveRecord provides. 
```
unless current_user.brew_logs.empty?
  current_user.brew_logs.select(:recipe_id).distinct.each do |log|
	  #code block
		  current_user.brew_logs.where(recipe_id: log.recipe.id).each do |log|
			  #code block
			end
```

.select returns only the database records that match your query. In this case I will only create objects from the brew_logs that match the recipe_id. In my case I only wanted a unique version of those arrays so chaining .distinct will return one object per unique recipe_id. 

### read the docs of the tools you are using!





