# Week 3 Comprehensive Assessment

Fork this repository, update this file to include your answers, and submit a pull request. You may refer to any notes, past projects, or external resources you want. The questions do not have to be completed in order.

### Models

1. I'm creating an app to keep track of bunnies. I already have a `bunnies` table, but I want to create a migration to add a "weight" column to it. What command should I run in my terminal to get started?

rails g migration AddWeightToBunnies
rake db:migrate


2. I just realized I misspelled the "weight" column in my migration, but I already ran `rake db:migrate`. What should I do to fix this? (give exact steps and/or commands to run)

# MY ANSWER
rake db:rollback
rails g migration AddWeightToBunnies # with weight spelled properly this time
rake db:migrate

# CORRECT
rake db:rollback
(modify existing migration)
rake db:migrate

# OR
(create a new migration)
rake db:migrate

3. My app has a `Bunny` model, and I want to find bunnies whose `color` attribute is `'white'`, sorted by their `name` attribute. What code should I write to do that?

Bunny.where(color: 'white').order(:name)


4. Now I want to find the specific bunny whose name is `'George'` (names are unique, so there should be only one).

Bunny.find_by(name: 'George')


5. I want to make sure nobunny, er, I mean nobody, can create a bunny without a name. What code should I add to my `Bunny` model to validate this?

# MY ANSWER
validates :bunny, presence: true

# CORRECT
validates :name, presence: true


### Controllers

1. My app is telling me there's an error in the `BunniesController`. What directory and filename should I look in?

app/controllers/bunnies_controller.rb


2. I'm in the `show` action of my `BunniesController` and I have the ID of a specific bunny in `params[:id]`. What line should I type to find the bunny with the correct ID, and assign it to a variable that my view can access?

# MY ANSWER
George = Bunny.find(:id)
Bunny.save

# CORRECT
@bunny = Bunny.find(params[:id])

3. I tried to update a bunny with the code `bunny.update(params[:bunny])`, but it gave me a "forbidden attributes error". Why is it telling me this, and what should I do (broadly speaking, no exact code needed) to fix the problem?

# MY ANSWER
You are trying to enter an attribute that is not allowed as a parameter. You should adjust your entry to one of the allowable parameters.


# CORRECT
Passing form parameters straight to the 'new' or update' methods is not allowed because it poses a security risk (because users might update any parameters that they want to).

To fix: create a (private) Strong Parameters filtering method in Controller to filter the params so they include only the attributes that should be updated by the user.


4. When I create or update a bunny in my controller, how can I find out whether the bunny saved successfully?

# MY ANSWER
Bunny.persisted?

# CORRECT
if @bunny.save
...success...
else
...failure...
end


5. Assuming my bunny saved successfully, what code should I write to redirect the user to the "show" page for the bunny, with a flash message indicating success?

# MY ANSWER
  def show
    @bunny = Bunny.find(params[:id])
  end

# CORRECT (any of below would work)
redirect_to @bunny # preferred
redirect_to bunny_path(@bunny)
redirect_to bunny_path(@bunny.id)

# WORKS, BUT MIGHT NOT ALWAYS WORK
redirect_to bunny_path

### Routes/Views

1. What line should I add to `config/routes.rb` to create a complete set of RESTful routes for a "bunnies" resource?

resources :bunnies


2. My app is telling me there's an error in the "show" view for bunnies. What directory and filename would that be located in?

app/views/bunnies/show.html.erb

3. I'm in the `index.html.erb` view and I've assigned a variable `@bunnies` to a collection of all my bunnies. What HTML/ERB code should I write to create an unordered list that shows each bunny's `name` attribute?

<ul> All Bunnies
<% @bunnies.each do |bunny| %>
<li>Name is <%= bunny.name %></li>
<% end %>
</ul>


# THESE TWO OPTIONS WOULD WORK
# BUT ABOVE IS PREFERRED

<%= render @bunnies %>

# OR

<%= render partial: 'bunny', collection: @bunnies %>

4. In one of my views, I want to create a link to the "show" path for a specific bunny that I have stored in the variable `bunny`. `rake routes` tells me that I have a standard `bunny_path` helper available. How do I create this link?

# MY ANSWER
<h1> <%= link_to 'See a specifc bunny', new_bunny_path %> </h1>

# CORRECT - any of below would work
<%= link_to 'anything', bunny %>
<%= link_to 'anything', bunny_path(bunny) %>
<%= link_to 'anything', bunny_path(bunny.id) %>


5. I've created a view partial called `_form.html.erb` and I want to render this partial into my "new" view. What HTML/ERB code should I write to do this?

# MY ANSWER
<%= render 'form', bunny: @bunny %>

# THIS WILL WORK, TOO
<%= render 'form' %>



