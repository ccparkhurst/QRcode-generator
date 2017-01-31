#README
This is a quick and dirty QR code generator using the rqr Ruby gem.

### Installation
Fork or clone this repo, run a bundle install, and fire up your Rails server
To generate the QR Codes you will use the rqrcode gem
```
gem install rqrcode
```

Or 

```
gem 'rqrcode', '~> 0.10.1'
bundle install
```


### Or build it yourself following the below steps.


Get your RQRCode up and running in **7 simple steps**. You'll want to take an existing Rails app that is up and running and do the following:

1 - Add 'rqr'code to your Gemfile and run a bundle install

```
gem 'rqrcode', '~> 0.10.1'
bundle install
```

2 - Create a new qr_codes controller from the terminal

```
rails g controller qr_codes new create
```

3 - Modify your environment.rb to require rqrcode <br /> 
<br />
  **config/environment.rb:**

```
# Load the Rails application.
require 'rqrcode'
require File.expand_path('../application', __FILE__)

# Initialize the Rails application.
Rails.application.initialize!
```

4 - Modify your routes to set up the routes for the controller you just created <br /><br />
  **config/routes.rb:**
```
Rails.application.routes.draw do
  resources :qr_codes, only: [:new, :create]
  root to: "qr_codes#new"
end
```

5 - Add code to the controller you just created <br /><br />
  **app/controllers/qr_codes_controller.rb:**
```
class QrCodesController < ApplicationController
  def new
  end

  def create
    @qr = RQRCode::QRCode.new( qr_code_params[:text], size: 4)
  end

private
  def qr_code_params
    params.require(:qr_code).permit(:text)
  end
end
```

6 - Create your views, starting with the new view for the QRCodes controller <br /><br />
  **app/views/qr_codes/new.html.erb:**

```
<%= form_for :qr_code, url: qr_codes_path do |f| %>
  <%= f.label :text %>
  <%= f.text_field :text %>
  <%= f.submit %>
<% end %>
```

Then modify your create view <br /><br />
  **app/views/qr_codes/create.html.erb:**
```
<table class="qr-code">
<% @qr.modules.each_index do |x| %>
  <tr>
  <% @qr.modules.each_index do |y| %>
   <% if @qr.dark?(x,y) %>
    <td class="black"/>
   <% else %>
    <td class="white"/>
   <% end %>
  <% end %>
  </tr>
<% end %>
</table>
<br />
<br />
<%= link_to "New QR Code", new_qr_code_path %>
```

7 - Finally, add some styling to your CSS so it looks like the QR Code you know and love <br /><br />
  **app/assets/stylesheets/application.css:**
```
table.qr-code {
  border-width: 0;
  border-style: none;
  border-color: #0000ff;
  border-collapse: collapse;
}
table.qr-code td {
  border-width: 0;
  border-style: none;
  border-color: #0000ff;
  border-collapse: collapse;
  padding: 0;
  margin: 0;
  width: 15px;
  height: 15px;
}
table.qr-code td.black { background-color: #000; }
table.qr-code td.white { background-color: #fff; }
```

### Serve it up! 
Run your rails server and test it out on localhost:3000
