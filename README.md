# Conviva Integration Guide for Salesforce Commerce Cloud (SFCC)

This guide describes the **step-by-step manual integration** process for **Salesforce Commerce Cloud (SFCC)** using the **Conviva standalone JS sensor** from the [`conviva-js-script-appanalytics`](https://github.com/Conviva/conviva-js-script-appanalytics) repository.

---

## Prerequisites

Before you begin, ensure you have the following:

- ✅ **CUSTOMER_KEY** provided by **Conviva** or your analytics admin
- ✅ **App Name & Version** for your shop (e.g., `"SFCC Shop"` / `"1.0.0"`)
- ✅ The **latest Conviva JS sensor** (Download from [conviva-js-script-appanalytics](https://github.com/Conviva/conviva-js-script-appanalytics))

---

## Setup

### 1. Download the JS Sensor

- Download the `convivaAppTracker.js` script file from the [conviva-js-script-appanalytics repo](https://github.com/Conviva/conviva-js-script-appanalytics).

### 2. Upload to SFCC

- Upload the `convivaAppTracker.js` file to your SFCC **Static** folder (e.g., `/static/conviva/`).

---

## Integration Steps

### 3. Include the Sensor

Add the following script tag in your site's **common head include** (e.g., `head.isml`), **before any other scripts**:

```html
<script src="${StaticURL}/conviva/convivaAppTracker.js"></script>
```

---

### 4. Initialize the Tracker

Immediately after including the sensor, add this snippet in `head.isml`:

```html
<script>
(function(p, i) {
    if (!p[i]) {
        p.GlobalConvivaNamespace = p.GlobalConvivaNamespace || [];
        p.GlobalConvivaNamespace.push(i);
        p[i] = function() {
            (p[i].q = p[i].q || []).push(arguments)
        };
        p[i].q = p[i].q || [];
    }
}(window, "apptracker"));
</script>

<script>
window.apptracker('convivaAppTracker',
  {     
    appId: 'YOUR_SFCC_APP_NAME',     
    convivaCustomerKey: 'YOUR_CUSTOMER_KEY',     
    appVersion: 'YOUR_APP_VERSION'   
  }
);
</script>
```

#### Parameters:
- `appId`: App identifier (e.g., `"SFCC Desktop Store"`)
- `convivaCustomerKey`: Your account key from Conviva Pulse
- `appVersion`: Semantic version string of the application (e.g., `"1.0.0"`)

---

### 5. Set the User ID (Optional but Recommended)

If your site has a logged-in customer ID (e.g., `customer.profile.customerNo`), set it **before any auto-collection**:

```html
<script>   
window.apptracker('setUserId', '${Customer.profile.customerNo}');
</script>
```

This ensures **all events are tagged** with your shopper’s ID.

---

### 6. Report Page Views

After your page is fully rendered, trigger the page view event:

```html
<script>   
// fire when content is ready   
window.apptracker('trackPageView');
</script>
```

> **Note:** For SPAs or componentized pages, you can call this on route/content changes. By default, it uses `document.title` unless a custom title is provided.

---

### 7. Track Custom Events

To track specific user interactions (e.g., **Add to Cart**), use the following snippet where appropriate:

```html
<script>   
window.apptracker('trackCustomEvent', {     
    name: 'add_to_cart',     
    data: {       
      productId: '${product.ID}',       
      price: '${product.priceModel.price}'     
    }   
});
</script>
```

---

## Additional Resources

- **Full Documentation & Examples**: [Conviva JS App Analytics Repository](https://github.com/Conviva/conviva-js-script-appanalytics)

---

## Support

For any questions or additional help, please contact your **Conviva support** 

---

