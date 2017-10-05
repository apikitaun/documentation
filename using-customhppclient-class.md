# Using CustomHttpClient

## 1. Prepare Startup.cs

If you are developing an MVC or MVC Api application, you must open _Startup.cs_ file and write the following code in _Configuration_ method:

    public partial class Startup

`    {`

`        public void Configuration(IAppBuilder app)`

`        {`

`            CustomHttpClient.SetServerCertificateValidation(`

                              `"CN=SSL-CERTIFICATE-SUBJECTNAME",@"client.cer");`

`            ConfigureAuth(app);`

`        }`



You must call static method _CustomHttpClient.SerServerCertificateValidation_ one time in your application. If you are developing a console application you can call such method at the begining of the _Main_ function in _Program.cs_

The parameters for SetServerCertificateValidation are the followings:

* _SSLCertificateSubjectName: _Certificate SubjectName of the SSL Certificate used for implements https in your server. This certificate is imported from _Machine Certificate Store_
* clientCertificatePath: Path to _.CER certificate _used by the client app to authenticate against the server

## 2. Use HttpClient instance

To use HttpClient in your code you must:

* Instanciate the class HttpClient using true if you want https communication
* Use one of the functions designed:
  * InvokeGet
  * InvokePost using string Content
  * InvokePost using object Content and internally serialized to Json
  * InvokePost&lt;T&gt; \(Generic Class\) using String Content
  * InvokePost&lt;T&gt; \(Generic Class\) using object Content and internally serialized to Json

The following code invoke a post rest service:

`        public async Task<string> TestStatus()`

`        {`

`            CustomHttpClient client = new CustomHttpClient(true);`

`            return await client.InvokeGet("localhost:8081", "api/Test/DoTest");`

`        }`

**NOTE: **_If you want to deserialize response to your custom class you must use GenericClass Methods. One example is the following_

`await client.InvokeGet<List<Customer>("localhost:8081","api/Customer");`

