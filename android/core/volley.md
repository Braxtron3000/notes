# Volley

volley uses http to make connections and schedule stuff using threads. It's recommended to create a requestqueue using a singleton so that it establishes one connection throughout the lifecycle of the activity. It is recommended not to use volley for images or videos or other large files. For that possibly look into java HTTPURLCONNECTION object.

implement into module gradle

```
implementation 'com.android.volley:volley:1.2.0'
```

here's what it looks like. make sure you use the correct url the api providers specify! I did this wrong for a while then realized it required http: at the front!

```
RequestQueue queue = Volley.newRequestQueue(context);

        //final String url = url_base + q + url_base_key;


        final JsonObjectRequest jsonObjectRequest = new JsonObjectRequest(Request.Method.GET, url, null, new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                Log.v("FUN", "RES: " + response.toString());
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                Log.v("FUN", "Err: " + error.getLocalizedMessage());
            }
        });

        Volley.newRequestQueue(context).add(jsonObjectRequest);
```