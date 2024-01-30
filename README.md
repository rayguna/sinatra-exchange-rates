# sinatra-exchange-rates

[Notes for this project are here.](https://learn.firstdraft.com/lessons/101)

Activities:

1. Initially, set the app.rb main route to the following to display the url:

```
get("/") do
  "
  <h1>Welcome to your Sinatra App!</h1>
  <p>Define some routes in app.rb</p>
  "
end
```

2. Sign up for a free API key from exchangerate.host.

3. Set up the secret API key in github by going to profile > settings > Codespaces > Codespaces secrets| New secret. Set the name for the secret key. Note the spelling. You will have to use the exact spelling to retrieve this key. Set to the appropriate repository (i.e., sinatra-exchange-rates).

4. Now that you have an API key, modify the get("/") do block as follows.

```
get("/") do

  # build the API url, including the API key in the query string
  api_url = "http://api.exchangerate.host/list?access_key=#{ENV["EXCHANGE_RATES_KEY"]}"

  # use HTTP.get to retrieve the API information
  raw_data = HTTP.get(api_url)

  # convert the raw request to a string
  raw_data_string = raw_data.to_s

  # convert the string to JSON
  parsed_data = JSON.parse(raw_data_string)

  # get the symbols from the JSON
  @symbols = parsed_data

  # render a view template where I show the symbols
  erb(:homepage)

end
```

In the above, you parse the retrieved API information into JSON and pass it to @symbols parameter. To view the contents of the JSON file, pass the @symbols parameter to homepage.erb, as follows.

Note that the url begins with http://, not https://. You will get an error message if it was set to https://.

```
<!DOCTYPE html>
<html>
  <head>
    <title>Target: Exchange Rates</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
  </head>

  <body>
    <h1>Currency pairs</h1>
<%=@symbols%>

...
```

The rest of the contents of homepage.erb is taken from the target page.

Here is the contents of the JSON file.

```
{"success"=>true, "terms"=>"https://currencylayer.com/terms", "privacy"=>"https://currencylayer.com/privacy", "currencies"=>{"AED"=>"United Arab Emirates Dirham", "AFN"=>"Afghan Afghani",...}
```

5. This project builds up from the previous projects:
- <a href="https://learn.firstdraft.com/lessons/111-sinatra-dice-dynamic-routes"> dynamic path segments</a>,

- APIs (including the <a href="https://learn.firstdraft.com/lessons/104-umbrella">Umbrella project</a>),

- and <a href="https://learn.firstdraft.com/lessons/104-umbrella#useful-methods">JSON parsing</a> 
