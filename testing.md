In your testing suite: 
    assert r.headers["location"] == "/account?updated=1"

remember to match the object of the key; location, with your endpoint's redirect. Mine was `/account`. I forgot I added the query string at the end :)

---

TemplateResponse with status_code=303 doesn't automatically add a Location header - only RedirectResponse does. You should not set status_code=303 on TemplateResponse calls.

