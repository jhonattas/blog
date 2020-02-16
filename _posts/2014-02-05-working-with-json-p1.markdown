---
layout: post
title:  "Android: Trabalhando com JSON"
date:   2014-02-05 19:01:13
categories: [android, tutoriais, json]
tags: [android, tutorial, json]
comments: true
image:
  background: witewall_3.png
---
Um exemplo de JSON.

A seguir temos o exemplo de JSON que será parseado neste tutorial. É um JSON muito simples que nos da uma lista de contatos, e que contem informações como: email, endereco, genero, e numeros de telefone.

Você pode obter este JSON acessando o link: [http://api.androidhive.info/contacts/] 

{% highlight json %}
{
    "contacts": [{
		"id": "c200",
		"name": "Ravi Tamada",
		"email": "ravi@gmail.com",
		"address": "xx-xx-xxxx,x - street, x - country",
		"gender" : "male",
		"phone": {
			"mobile": "+91 0000000000",
			"home": "00 000000",
			"office": "00 000000"
		}
	},
	{
		"id": "c201",
		"name": "Johnny Depp",
		"email": "johnny_depp@gmail.com",
		"address": "xx-xx-xxxx,x - street, x - country",
		"gender" : "male",
		"phone": {
			"mobile": "+91 0000000000",
			"home": "00 000000",
			"office": "00 000000"
		}
    }]
}
{% endhighlight %}

### A diferença entre [ e { - (colchetes e chaves)

Em geral todos os nós JSON vão começar com um colchete ou com uma chave. A diferença entre [ e { é, que o colchete ( <b>[</b> ) representa o inicio de uma lista de nós. Enquanto o colchete representa um objeto. Assim, ao acessar estes nós, precisaremos de métodos apropriados para acessar os dados.

![json parsing structor!]({{ site.url }}/assets/images/json_structor.png).

### Criando um novo projeto Android

Vamos iniciar criando um novo projeto no Android

1 . Crie um projeto novo

2 . Como estamos buscando o JSON realizando chamadas HTTP, nós precisaremos adicionar uma `Permissão de INTERNET` no seu arquivo AndroidManifest.xml. Abra este arquivo, e adicione a seguinte configuração.

{% highlight xml %}
<uses-permission android:name="android.permission.INTERNET" />
{% endhighlight %}

Exemplo:
{% highlight xml %}
AndroidManifest.xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="info.androidhive.jsonparsing"
    android:versionCode="1"
    android:versionName="1.0" >
 
    <uses-sdk
        android:minSdkVersion="8"
        android:targetSdkVersion="18" />
     
    <!-- Internet permission -->
    <uses-permission android:name="android.permission.INTERNET" />
 
    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <activity
            android:label="@string/app_name"
            android:name="info.androidhive.jsonparsing.MainActivity" >
            <intent-filter >
                <action android:name="android.intent.action.MAIN" />
 
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
         
        <!-- Single List Item Activity -->
        <activity
            android:label="Contact"
            android:name="info.androidhive.jsonparsing.SingleContactActivity" >
        </activity>
    </application>
 
</manifest>
{% endhighlight %}

### A classe para manipular as chamadas JSON

Estou criando uma classe separada para lidar com as chamadas HTTP. Essa classe é responsavel por fazer a chamada HTTP, e também obter uma resposta.

3 . Crie uma classe chamada <b>ServiceHandler.java</b> e adicione o código a seguir. Nesta classe será chamado o método <b>makeServiceCall(String url, int method, List params)</b> afim de realizar a chamada HTTP.

Parametros: 	

`url` - A url que realiza a chamada http

`method` - o método HTTP podendo ser GET ou POST. Podemos passar <b>ServiceHandler.GET</b> ou <b>ServiceHandler.POST</b> como valor.

`params` - Qualquer parametros que você desejar submeter a url. Este é opcional.

{% highlight java%}
ServiceHandler.java
package info.androidhive.jsonparsing;
 
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.List;
 
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.client.utils.URLEncodedUtils;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;
 
public class ServiceHandler {
 
    static String response = null;
    public final static int GET = 1;
    public final static int POST = 2;
 
    public ServiceHandler() {
 
    }
 
    /**
     * Making service call
     * @url - url to make request
     * @method - http request method
     * */
    public String makeServiceCall(String url, int method) {
        return this.makeServiceCall(url, method, null);
    }
 
    /**
     * Making service call
     * @url - url to make request
     * @method - http request method
     * @params - http request params
     * */
    public String makeServiceCall(String url, int method,
            List<NameValuePair> params) {
        try {
            // http client
            DefaultHttpClient httpClient = new DefaultHttpClient();
            HttpEntity httpEntity = null;
            HttpResponse httpResponse = null;
             
            // Checking http request method type
            if (method == POST) {
                HttpPost httpPost = new HttpPost(url);
                // adding post params
                if (params != null) {
                    httpPost.setEntity(new UrlEncodedFormEntity(params));
                }
 
                httpResponse = httpClient.execute(httpPost);
 
            } else if (method == GET) {
                // appending params to url
                if (params != null) {
                    String paramString = URLEncodedUtils
                            .format(params, "utf-8");
                    url += "?" + paramString;
                }
                HttpGet httpGet = new HttpGet(url);
 
                httpResponse = httpClient.execute(httpGet);
 
            }
            httpEntity = httpResponse.getEntity();
            response = EntityUtils.toString(httpEntity);
 
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        } catch (ClientProtocolException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
         
        return response;
 
    }
}
{% endhighlight %}

4 . Estou adicionando um listview para mostrar os dados do JSON parseados. Abra o arquivo de sua activity principal e adicione o elemento listview.

{% highlight xml%}
activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical">
    <!-- Main ListView 
         Always give id value as list(@android:id/list)
    -->
    <ListView
        android:id="@android:id/list"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
 
</LinearLayout>
{% endhighlight %}

5 . Nós precisamos de outro arquivo de layout que vai exibir uma simples linha no listview. Crie um outro arquivo de layout chamado <b>list_item.xml</b> com o seguinte conteúdo:

{% highlight java%}

list_item.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="10dp"
    android:paddingLeft="10dp"
    android:paddingRight="10dp" >
 
    <!-- Name Label -->
 
    <TextView
        android:id="@+id/name"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:paddingBottom="2dip"
        android:paddingTop="6dip"
        android:textColor="#43bd00"
        android:textSize="16sp"
        android:textStyle="bold" />
 
    <!-- Email label -->
    <TextView
        android:id="@+id/email"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:paddingBottom="2dip"
        android:textColor="#acacac" />
 
    <!-- Mobile number label -->
    <TextView
        android:id="@+id/mobile"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:gravity="left"
        android:text="Mobile: "
        android:textColor="#5d5d5d"
        android:textStyle="bold" />
 
</LinearLayout>
{% endhighlight %}

6 . Na minha activity principal (classe <b>MainActivity.java</b>), declarei todos os nomes de nós do JSON e criei uma instancia da list view. 

{% highlight java %}
MainActivity.java
public class MainActivity extends ListActivity {
 
    private ProgressDialog pDialog;
 
    // URL to get contacts JSON
    private static String url = "http://api.androidhive.info/contacts/";
 
    // JSON Node names
    private static final String TAG_CONTACTS = "contacts";
    private static final String TAG_ID = "id";
    private static final String TAG_NAME = "name";
    private static final String TAG_EMAIL = "email";
    private static final String TAG_ADDRESS = "address";
    private static final String TAG_GENDER = "gender";
    private static final String TAG_PHONE = "phone";
    private static final String TAG_PHONE_MOBILE = "mobile";
    private static final String TAG_PHONE_HOME = "home";
    private static final String TAG_PHONE_OFFICE = "office";
 
    // contacts JSONArray
    JSONArray contacts = null;
 
    // Hashmap for ListView
    ArrayList<HashMap<String, String>> contactList;
 
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        contactList = new ArrayList<HashMap<String, String>>();
 
        ListView lv = getListView();
    }
}
{% endhighlight %}

### Parseando os dados do JSON
7 . Como estaremos recuperando um JSON por uma chamada http, eu estou adicionando um método assincrono para fazer chamadas http em uma thread no background. Adicione o método a seguir na sua classe principal.

No método <b>onPreExecute()</b> eu mostro um dialogo de progresso antes de realizar a atual chamada http.

No método <b>doInBackground()</b> eu chamei o método <b>makeServiceCall()</b> para recuperar o json a partir de uma url, uma vez parseado este JSON ele é adicionado ao HashMap para mostrar os resultados em uma ListView.

No método <b>onPostExecute()</b> eu criei um ListAdapter, o associei a um ListView e dispensei o alerta de progresso.

Note tambem que eu utilizei o método <b>getJSONArray()</b> ou o <b>getJSONObject()</b> dependendo do tipo de nó.

{% highlight java %}
MainActivity.java
public class MainActivity extends ListActivity {
    .
    .
    .
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        .
        .
        .
 
        // Calling async task to get json
        new GetContacts().execute();
    }
 
    /**
     * Async task class to get json by making HTTP call
     * */
    private class GetContacts extends AsyncTask<Void, Void, Void> {
 
        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            // Showing progress dialog
            pDialog = new ProgressDialog(MainActivity.this);
            pDialog.setMessage("Please wait...");
            pDialog.setCancelable(false);
            pDialog.show();
 
        }
 
        @Override
        protected Void doInBackground(Void... arg0) {
            // Creating service handler class instance
            ServiceHandler sh = new ServiceHandler();
 
            // Making a request to url and getting response
            String jsonStr = sh.makeServiceCall(url, ServiceHandler.GET);
 
            Log.d("Response: ", "> " + jsonStr);
 
            if (jsonStr != null) {
                try {
                    JSONObject jsonObj = new JSONObject(jsonStr);
                     
                    // Getting JSON Array node
                    contacts = jsonObj.getJSONArray(TAG_CONTACTS);
 
                    // looping through All Contacts
                    for (int i = 0; i < contacts.length(); i++) {
                        JSONObject c = contacts.getJSONObject(i);
                         
                        String id = c.getString(TAG_ID);
                        String name = c.getString(TAG_NAME);
                        String email = c.getString(TAG_EMAIL);
                        String address = c.getString(TAG_ADDRESS);
                        String gender = c.getString(TAG_GENDER);
 
                        // Phone node is JSON Object
                        JSONObject phone = c.getJSONObject(TAG_PHONE);
                        String mobile = phone.getString(TAG_PHONE_MOBILE);
                        String home = phone.getString(TAG_PHONE_HOME);
                        String office = phone.getString(TAG_PHONE_OFFICE);
 
                        // tmp hashmap for single contact
                        HashMap<String, String> contact = new HashMap<String, String>();
 
                        // adding each child node to HashMap key => value
                        contact.put(TAG_ID, id);
                        contact.put(TAG_NAME, name);
                        contact.put(TAG_EMAIL, email);
                        contact.put(TAG_PHONE_MOBILE, mobile);
 
                        // adding contact to contact list
                        contactList.add(contact);
                    }
                } catch (JSONException e) {
                    e.printStackTrace();
                }
            } else {
                Log.e("ServiceHandler", "Couldn't get any data from the url");
            }
 
            return null;
        }
 
        @Override
        protected void onPostExecute(Void result) {
            super.onPostExecute(result);
            // Dismiss the progress dialog
            if (pDialog.isShowing())
                pDialog.dismiss();
            /**
             * Updating parsed JSON data into ListView
             * */
            ListAdapter adapter = new SimpleAdapter(
                    MainActivity.this, contactList,
                    R.layout.list_item, new String[] { TAG_NAME, TAG_EMAIL,
                            TAG_PHONE_MOBILE }, new int[] { R.id.name,
                            R.id.email, R.id.mobile });
 
            setListAdapter(adapter);
        }
 
    }
 
}
{% endhighlight %}

Se você executar o projeto, você pode ver os dados do JSON preencherem a lista. O código para que o clique em um item da lista seja reconhecido, iniciando uma activity nova, pode ser encontrado no código disponibilizado para download, mas não sera explicado aqui, pois não é o foco do artigo.

![android json parsing list view]({{ site.url }}/assets/images/json_parsing_listview.png).

### Codigo completo

{% highlight java %}
MainActivity.java
package info.androidhive.jsonparsing;
 
import info.androidhive.jsonparsing.R;
import java.util.ArrayList;
import java.util.HashMap;
 
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;
 
import android.app.ListActivity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.AsyncTask;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.AdapterView;
import android.widget.AdapterView.OnItemClickListener;
import android.widget.ListAdapter;
import android.widget.ListView;
import android.widget.SimpleAdapter;
import android.widget.TextView;
 
public class MainActivity extends ListActivity {
 
    private ProgressDialog pDialog;
 
    // URL to get contacts JSON
    private static String url = "http://api.androidhive.info/contacts/";
 
    // JSON Node names
    private static final String TAG_CONTACTS = "contacts";
    private static final String TAG_ID = "id";
    private static final String TAG_NAME = "name";
    private static final String TAG_EMAIL = "email";
    private static final String TAG_ADDRESS = "address";
    private static final String TAG_GENDER = "gender";
    private static final String TAG_PHONE = "phone";
    private static final String TAG_PHONE_MOBILE = "mobile";
    private static final String TAG_PHONE_HOME = "home";
    private static final String TAG_PHONE_OFFICE = "office";
 
    // contacts JSONArray
    JSONArray contacts = null;
 
    // Hashmap for ListView
    ArrayList<HashMap<String, String>> contactList;
 
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        contactList = new ArrayList<HashMap<String, String>>();
 
        ListView lv = getListView();
 
        // Listview on item click listener
        lv.setOnItemClickListener(new OnItemClickListener() {
 
            @Override
            public void onItemClick(AdapterView<?> parent, View view,
                    int position, long id) {
                // getting values from selected ListItem
                String name = ((TextView) view.findViewById(R.id.name))
                        .getText().toString();
                String cost = ((TextView) view.findViewById(R.id.email))
                        .getText().toString();
                String description = ((TextView) view.findViewById(R.id.mobile))
                        .getText().toString();
 
                // Starting single contact activity
                Intent in = new Intent(getApplicationContext(),
                        SingleContactActivity.class);
                in.putExtra(TAG_NAME, name);
                in.putExtra(TAG_EMAIL, cost);
                in.putExtra(TAG_PHONE_MOBILE, description);
                startActivity(in);
 
            }
        });
 
        // Calling async task to get json
        new GetContacts().execute();
    }
 
    /**
     * Async task class to get json by making HTTP call
     * */
    private class GetContacts extends AsyncTask<Void, Void, Void> {
 
        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            // Showing progress dialog
            pDialog = new ProgressDialog(MainActivity.this);
            pDialog.setMessage("Please wait...");
            pDialog.setCancelable(false);
            pDialog.show();
 
        }
 
        @Override
        protected Void doInBackground(Void... arg0) {
            // Creating service handler class instance
            ServiceHandler sh = new ServiceHandler();
 
            // Making a request to url and getting response
            String jsonStr = sh.makeServiceCall(url, ServiceHandler.GET);
 
            Log.d("Response: ", "> " + jsonStr);
 
            if (jsonStr != null) {
                try {
                    JSONObject jsonObj = new JSONObject(jsonStr);
                     
                    // Getting JSON Array node
                    contacts = jsonObj.getJSONArray(TAG_CONTACTS);
 
                    // looping through All Contacts
                    for (int i = 0; i < contacts.length(); i++) {
                        JSONObject c = contacts.getJSONObject(i);
                         
                        String id = c.getString(TAG_ID);
                        String name = c.getString(TAG_NAME);
                        String email = c.getString(TAG_EMAIL);
                        String address = c.getString(TAG_ADDRESS);
                        String gender = c.getString(TAG_GENDER);
 
                        // Phone node is JSON Object
                        JSONObject phone = c.getJSONObject(TAG_PHONE);
                        String mobile = phone.getString(TAG_PHONE_MOBILE);
                        String home = phone.getString(TAG_PHONE_HOME);
                        String office = phone.getString(TAG_PHONE_OFFICE);
 
                        // tmp hashmap for single contact
                        HashMap<String, String> contact = new HashMap<String, String>();
 
                        // adding each child node to HashMap key => value
                        contact.put(TAG_ID, id);
                        contact.put(TAG_NAME, name);
                        contact.put(TAG_EMAIL, email);
                        contact.put(TAG_PHONE_MOBILE, mobile);
 
                        // adding contact to contact list
                        contactList.add(contact);
                    }
                } catch (JSONException e) {
                    e.printStackTrace();
                }
            } else {
                Log.e("ServiceHandler", "Couldn't get any data from the url");
            }
 
            return null;
        }
 
        @Override
        protected void onPostExecute(Void result) {
            super.onPostExecute(result);
            // Dismiss the progress dialog
            if (pDialog.isShowing())
                pDialog.dismiss();
            /**
             * Updating parsed JSON data into ListView
             * */
            ListAdapter adapter = new SimpleAdapter(
                    MainActivity.this, contactList,
                    R.layout.list_item, new String[] { TAG_NAME, TAG_EMAIL,
                            TAG_PHONE_MOBILE }, new int[] { R.id.name,
                            R.id.email, R.id.mobile });
 
            setListAdapter(adapter);
        }
 
    }
 
}
{% endhighlight %}

traduzido de: [http://www.androidhive.info/]

[http://www.androidhive.info/]:    http://www.androidhive.info/2012/01/android-json-parsing-tutorial/
[http://api.soucriador.com.br/contacts/]:    http://api.soucriador.com.br/contacts/
