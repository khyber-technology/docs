# Inbound Leads API (source markers)

The System uses form data to create a new client lead.

Form data is sent to the server via a POST request.

URL: `https://system.yourdomain.co.uk/f/inbound`

Matched fields are:

- `starting_status` (ID)
- `campaign_id`
- `source_marker`
- `title`
- `first_name`
- `last_name`
- `mobile`
- `email_address`
- `date_of_birth` formatted to `YYYY-MM-DD`

All the above values will be stored in their respective fields in the database.

Any other values will be store in the database as `additional_data` in JSON format.
This shows as '**Source Information**' on the client's record.

Any additional data will be modified to title case, and spaced. For example, if you pass
`debt_level` in the form data, it will be shown as `Debt Level` on the client's record.

The system also accepts redirection URLs:

- `success_url`
- `fail_url`

Including these fields will redirect the user to the specified URL after the form is submitted.

IF YOU DO NOT include these, the system will respond with a JSON response.

Success:

```json
{
  "status": "success",
  "message": "Client added"
}
```

Fail:

```json
{
  "status": "error",
  "message": "Client not added"
}
```

Here's an example of a form setup:

```html
<form action="https://system.yourdomain.co.uk/f/inbound" method="POST">

    <label for="title">Title</label>
    <select name="title" id="title">
        <option value="Mr">Mr</option>
        <option value="Mrs">Mrs</option>
        <option value="Miss">Miss</option>
        <option value="Ms">Ms</option>
    </select>

    <label for="first_name">First Name</label>
    <input type="text" name="first_name" id="first_name">

    <label for="last_name">Last Name</label>
    <input type="text" name="last_name" id="last_name">

    <label for="mobile">Contact Number</label>
    <input type="text" name="mobile" id="mobile">

    <label for="email_address">Email Address</label>
    <input type="email" name="email_address" id="email_address">

    <label for="date_of_birth">Date of Birth</label>
    <input type="date" name="date_of_birth" id="date_of_birth">
    
    <label for="debt_level">Debt Level</label>
    <input type="number" name="debt_level" id="debt_level">

    <input type="hidden" name="starting_status" value="1">
    <input type="hidden" name="campaign_id" value="1">
    <input type="hidden" name="source_marker" value="Your Lead Name">
    <input type="hidden" name="success_url" value="https://yourwebsite.co.uk/success">
    <input type="hidden" name="fail_url" value="https://yourwebsite.co.uk/fail">

    <button type="submit">Submit</button>
    
</form>

<!--  input debt_level will be added to additional_data in the database -->
```

Here's an example of a JavaScript fetch request:

```javascript
function post_to_system(
    title,
    first_name,
    last_name,
    mobile,
    email_address,
    date_of_birth,
    starting_status,
    campaign_id,
    source_marker
) {

    const form = new FormData();

    form.append('title', title);
    form.append('first_name', first_name);
    form.append('last_name', last_name);
    form.append('mobile', mobile);
    form.append('email_address', email_address);
    form.append('date_of_birth', date_of_birth);

    form.append('starting_status', starting_status);
    form.append('campaign_id', campaign_id);
    form.append('source_marker', source_marker);

    fetch('https://system.yourdomain.co.uk/f/inbound', {
        method: 'POST',
        headers: {
            'cors': 'no-cors',
            'Content-Type': 'form-data'
        },
        body: form
    })

}
```

**NOTE** When using the fetch API, you must use the `FormData` object to send form data. You also must set
the `Content-Type` header to `form-data`, and the `cors` header to `no-cors`.

`no-cors` will mean that you will not get a response back from the server; this is by design. It's recommended to use
the form method shown above this JavaScript example.
