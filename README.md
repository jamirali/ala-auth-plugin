# ala-auth-plugin [![Build Status](https://travis-ci.org/AtlasOfLivingAustralia/ala-auth-plugin.svg?branch=master)](https://travis-ci.org/AtlasOfLivingAustralia/ala-auth-plugin)
## Usage
```
compile "org.grails.plugins:ala-auth:3.1.2"
```

## Description
ALA authentication/authorization Grails 3 plugin interface to CAS.  The Grails 2 version of this plugin can
be found on the grails2 branch.

## Usage

### Setup CAS Authentication for your app

In your `application.yml` or `application.groovy` you should define the following
properties:

```groovy
security {
    cas {
        appServerName = 'http://devt.ala.org.au:8080' # or similar, up to the request path part
        uriFilterPattern = '/paths/.*,/that,/require/.*,/auth.*'
        uriExclusionFilterPattern = '/assets/.*,/images/.*,/css/.*,/js/.*,/less/.*' # this is the default value
        authenticateOnlyIfLoggedInPattern =  '/optional-auth/.*'
    }
}
```

**NOTE** If setting `security.cas.appServerName` only and a scheme / port number is not specified: ensure that the app
server (eg Tomcat) is receiving the correct remote scheme / port from any reverse proxy (eg by using the AJP protocol
or enabling the Tomcat Remote IP Valve and the appropriate headers from the RP) otherwise the CAS filter will get
confused trying to generate the service url for the CAS callback.

The remaining properties should have sensible default values that are provided by this plugin.  You can
override these if you wish, however:

```yaml
security:
    cas:
        casServerName: https://auth.ala.org.au
        casServerUrlPrefix: https://auth.ala.org.au/cas
        loginUrl: https://auth.ala.org.au/cas/login
        logoutUrl: https://auth.ala.org.au/cas/logout
        bypass: false
        roleAttribute: authority
        ignoreCase: true
        authCookieName: ALA-Auth
```

`ala-cas-client` v2.3+ will now get the context path from the Servlet Context, so that property is
no longer required.

### (Optional) Add role based authorization using the @AlaSecured annotation

On a Grails controller, you can use the @AlaSecured annotation to do role based authorization for
Grails actions.

### Use a different user details location

You can change the base address of the UserDetails web services by overriding the following config value:

```groovy
userDetails.url = 'https://auth.ala.org.au/userdetails/'
```

### Migration from 1.x

See [this page](https://github.com/AtlasOfLivingAustralia/ala-auth-plugin/wiki/1.x-Migration-Guide) on the wiki for steps to upgrade from 1.x.

## Changelog
- **Version 3.2.3** (10/02/2021):
  - Updated `loginLogout`, supports Grails 3.x apps
- **Version 3.1.3** (10/02/2021):
  - Updated `loginLogout`, supports Grails 3.x apps
- **Version 3.0.5** (10/02/2021):
  - Updated `loginLogout`, supports Grails apps on ala-auth-plugin 3.0.x
- **Version 2.1.6** (10/02/2021):
  - Updated `loginLogout`, supports Grails 2.x apps
  
- **Version 3.1.0**:
  - Updates for ALA CAS 5
  - Update ALA CAS client
  - Update userdetails-service-client
  - Always use the CAS HttpServletRequestFilter to put the CAS principal in the request scope if it's available.
- **Version 3.0.3**:
  - Fix @Cacheable annotations to use the Grails versions instead of Spring
- **Version 3.0.2**:
  - Fix CAS filter registration order WRT the Grails Character Encoding filter.
- **Version 3.0.1**:
  - Support both `authenticateOnlyIfLoggedInPattern` and `authenticateOnlyIfLoggedInFilterPattern` properties for the only if previously logged in filter.
- **Version 3.0.0**:
  - Upgrade to Grails 3
  - Use userdetails-service-client in preference to HttpWebService class
  - Use ala-cas-client 2.3 changes
  - Move servlet context init-param and filter setup into Spring, allowing them to use properties directly from Application.groovy
- **Version 1.3** (07/05/2015):
  - Fixed several URL encoding issues
  - Add support for extra user properties in userdetails web services
  - Add missing config.userDetailsById.bulkPath default setting
- **Version 1.2** (24/02/2015):
  - Excludes the servlet-api dependency from being resolved as a dependency in the host app
- **Version 1.1** (23/02/2015):
  - Added `loginLogout` taglib method
- **Version 1.0** (18/02/2015):
  - Initial release.
