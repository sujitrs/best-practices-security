# best-practices-security

# Add in build.gradle

       `compile group: 'com.github.ulisesbocchio', name: 'jasypt-spring-boot-starter', version: '2.1.1'`
       
# Add Annotation SpringBoot Application Java File 

`@EnableEncryptableProperties`


# Create encrypted value for database password

Download jasypt from [here](http://www.jasypt.org/download.html)

```
#jasypt-1.9.2-dist\jasypt-1.9.2\bin> ./encrypt input=nsdl1234 password=MY_SECRET

----ENVIRONMENT-----------------
Runtime: Oracle Corporation OpenJDK 64-Bit Server VM 11.0.2+9

----ARGUMENTS-------------------
input: nsdl1234
password: MY_SECRET
----OUTPUT----------------------
sTU0WwEA45K/jHxcswowpRxE0xU2h4Vv
```
In above section `nsdl1234` is the value that you want to encrypt and `MY_SECRET` is the key for encryption whereas `sTU0WwEA45K/jHxcswowpRxE0xU2h4Vv` is the encrypted value that can be used.

# For Development and Test environment

### Windows
1. Set in environment variable: `set JASYPT_ENCRYPTOR_PASSWORD=MY_SECRET`
2. Verify its set: `echo %JASYPT_ENCRYPTOR_PASSWORD%`

### Unix
1. Set in environment variable: `export JASYPT_ENCRYPTOR_PASSWORD=MY_SECRET`
2. Verify its set: `echo $JASYPT_ENCRYPTOR_PASSWORD`

3. Execute the Spring Boot jar with `java -Djdbc.pass=ENC(sTU0WwEA45K/jHxcswowpRxE0xU2h4Vv)  -Djdbc.url=jdbc:postgresql://localhost:5434/ -jar ms-oauth2-server`


# For Docker

```
>docker run -e JASYPT_ENCRYPTOR_PASSWORD=MY_SECRET -p 8888:8888 org.sj/ms-oauth2-server -Djdbc.pass=ENC(sTU0WwEA45K/jHxcswowpRxE0xU2h4Vv) -Djdbc.url=jdbc:postgresql://10.0.75.1:5434/ org.sj.msoauth2server.MsOauth2ServerApplication
```


# For Production environment

Avoid exposing the password in the command line of jar execution, since you can query the processes with ps, previous commands with history, etc etc. You could:

1. Create a script like this: `touch setEnv.sh`
2. Edit setEnv.sh to export the `JASYPT_ENCRYPTOR_PASSWORD` variable

```
#!/bin/bash
export JASYPT_ENCRYPTOR_PASSWORD=MY_SECRET
```

3. Execute the file with `. setEnv.sh`
4. Run the app in background `` &
5. Delete the file `setEnv.sh`
6. Unset the previous environment variable with: `unset JASYPT_ENCRYPTOR_PASSWORD`
