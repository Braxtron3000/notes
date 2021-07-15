# Volley

volley uses http to make connections and schedule stuff using threads. It's recommended to create a requestqueue using a singleton so that it establishes one connection throughout the lifecycle of the activity.

here's what it looks like

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


        //queue.add(stringRequest);
        Volley.newRequestQueue(context).add(jsonObjectRequest);
```