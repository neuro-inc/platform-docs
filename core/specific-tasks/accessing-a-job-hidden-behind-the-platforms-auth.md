# Accessing a job protected by platform SSO

Here are the general steps for launching a job and accessing it through the platform's Auth.

### Generating a service account

Generate a service account via `apolo service-account create --name <sa-name>`. Make sure to store the **Role** and the **Auth token** - you will use them to authenticate your requests.

#### Example:

```
    $ apolo service-account create --name test
     Id               service-account-b41aa732-4bb5-45e4-94c1-a078ca013255
     Name             test
     Role             janedoe/service-accounts/test
     Owner            janedoe
     Default cluster  default
     Created at       now
     
    Full token with cluster and API url embedded (this value can be used as NEURO_PASSED_CONFIG environment variable):
    eyJ0b2tlbi<hidden>SJ9
    
    Just auth token (this value can be passed to apolo config login-with-token):
    eyJhbGciOi<hidden>Np0
    
    Save it to some secure place, you will be unable to retrieve it later!
```

In this case, `janedoe/service-accounts/test` is the needed role and\
`eyJhbGciOi<hidden>Np0` is the needed authentication token.

### Starting the required job

Start the job you want to access later.

#### Example:

```
$ apolo run --http 8080 --name mytestjob python -- python -m http.server 8080
```

### Share access to the job with the service account

Share access to the job with the service account role from step 1 by using the \
`apolo acl grant job:<job-id-or-name> <role-name> read` command.

#### Example:

```
$ apolo acl grant job:mytestjob janedoe/service-accounts/test read
```

### Using the auth token

Use the token from step 1 to authenticate the request with header via the `"cookie: dat=<token-here>;"` command.

#### Example:

```
    $ curl https://mytestjob--janedoe.jobs.default.org.neu.ro -H "cookie: dat=eyJhbGciOi<hidden>Np0"
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>Directory listing for /</title>
    </head>
    <body>
    <h1>Directory listing for /</h1>
    <hr>
    <ul>
    <li><a href=".dockerenv">.dockerenv</a></li>
    <li><a href="bin/">bin/</a></li>
    <li><a href="boot/">boot/</a></li>
    ....
```
