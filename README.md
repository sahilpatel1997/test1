## edX Enterprise Integration

The edX Enterprise API is part of edX for Business, which provides
corporations and other enterprises the ability to provide their
employees with access to the thousands of high-quality online courses on
the edX platform. An enterprise uses the edX Enterprise API to retrieve
its custom course catalog and display the course information in the
enterprise LMS or another system.

### To obtain your API credentials, complete the following steps.

1. Register at http://www.edx.org by creating a new user account for your company. The name you enter in the Public Username field must match the name of your company. For example, if your company is named Example, Inc., enter a name like Example or ExampleInc in the Public Username field.
2. After you have created an account, go to http://courses.edx.org/api-admin and fill out the API Access Request form. The name you enter in the Company Name field must be identical to the Public Username that you used to register on edx.org. For example, if your Public Username is ExampleInc, the Company Name must also be ExampleInc.
3. After you submit the API Access Request form, edX will create a course catalog for you and then send you an email describing how to generate your client identifier and client secret string. You can then use the client identifier and secret string to access your course catalog using the edX Enterprise API.

### Installation

1.  Put edx_enterprise_api app to edx-platform -&gt; openedx -&gt; features

2.  Add json config file:
    ```python
    EDX_ETERPRISE_API_CLIENT_ID’: " Your edX Enterprise Client Id",
    EDX_ETERPRISE_API_CLIENT_SECRET’: "Your edX Enterprise Client Secret",
    EDX_ETERPRISE_ACCESS_TOKEN_API’: "https://api.edx.org/oauth2/v1/access_token",
    EDX_ETERPRISE_CLIENT_CATALOG_DETAIL_API’: "https://api.edx.org/enterprise/v1/enterprise-catalogs/”,
    EDX_ETERPRISE_COURSE_DETAIL_API’:"https://api.edx.org/enterprise/v1/enterprise-catalogs/",
    ```
3.  Add in to the settings file (i.e. in edx-platform/lms/envs/common.py):
    ```python
    INSTALLED_APPS += ('edx_enterprise_api',)
    MAKO_TEMPLATE_DIRS_BASE +=[OPENEDX_ROOT / 'features' / 'edx_enterprise_api']
    'ENABLE_COURSE_DISCOVERY': True
    'ENABLE_COURSEWARE_SEARCH': True
    ```
4.  Add to LMS URLs (i.e. in edx-platform/lms/urls.py):
    ```python
    url(r'', include('openedx.features.edxorg\_courses.urls')),
    ```
5.  In /lms/templates/discovery/course\_card.underscore change second
    ```html
    <% if (org == 'edX Courses') { %>
        <a href="/courses/<%- course %>/course_about">
    <% } else { %>
        <a href="/courses/<%- course %>/about">
    <% } %>
    ```

6. Apply migration

7. Run in the console:
    ```python
    sudo -H -u edxapp bash
    source ~/edxapp_env 
    cd ~/edx-platform/
    python manage.py lms set_edxorg_courses --settings=”production”
