# My Code Snippets #

## API REST ##

### Importing ###

- compile "com.squareup.retrofit2:retrofit:2.3.0"	
- compile "com.squareup.retrofit2:converter-gson:2.3.0"
- compile "com.squareup.okhttp3:okhttp:3.9.1"
- compile "com.squareup.okhttp3:logging-interceptor:3.9.1"
- compile "com.facebook.stetho:stetho-okhttp3:1.5.0"
- compile "io.reactivex.rxjava2:rxjava:2.1.9"
- compile "io.reactivex.rxjava2:rxandroid:2.0.2"

### Defining Route ###

buildConfigField 'String', 'ROUTE_API', '"http://..."'

### Creating Retrofit Instance ###

        val clientBuilder = OkHttpClient().newBuilder().addNetworkInterceptor(StethoInterceptor())

        if (BuildConfig.DEBUG) {
            val httpInterceptor = HttpLoggingInterceptor()
            httpInterceptor.setLevel(HttpLoggingInterceptor.Level.BODY)

            clientBuilder.addInterceptor(httpInterceptor)
                    .addInterceptor(
                            object : Interceptor {
                                @Throws(IOException::class)
                                override fun intercept(chain: Interceptor.Chain): Response {
                                    val original = chain.request()
                                    val request = original.newBuilder()
                                            .method(original.method(), original.body())
                                            .build()
                                    return chain.proceed(request)
                                }
                            }
                    )
        }

        return Retrofit.Builder()
                .addConverterFactory(GsonConverterFactory.create())
                .addCallAdapterFactory(RxJava2CallAdapterFactory.create())
                .baseUrl(BuildConfig.ROUTE_API)
                .client(clientBuilder.build())
                .build()
