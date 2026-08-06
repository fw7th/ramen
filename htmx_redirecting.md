```
response = Response(status_code=200)
response.headers["HX-Redirect"] = "/signup"
return response
```

These three lines would tell htmx to do a full redirect to /signup
Unlike a normal HTTP 302 redirect (which HTMX would just swap into the target element), the HX-Redirect header instructs the HTMX client to navigate the entire browser window to that URL -> so it behaves like a regular link click.
