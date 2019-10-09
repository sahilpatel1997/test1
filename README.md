*edX Enterprise Integration*

The edX Enterprise API is part of edX for Business, which provides
corporations and other enterprises the ability to provide their
employees with access to the thousands of high-quality online courses on
the edX platform. An enterprise uses the edX Enterprise API to retrieve
its custom course catalog and display the course information in the
enterprise LMS or another system.

Setup edxorg\_courses app

1.  Put edxorg\_courses app to edx-platform -&gt; openedx -&gt; features

2.  In lms.env.json add below flags:
    > ‘EDXORG\_API\_CLIENT\_ID’: " Your edX Enterprise Client Id",\
    > ‘EDXORG\_API\_CLIENT\_SECRET’: "Your edX Enterprise Client Secret
    > ",\
    > ‘EDXORG\_ACCESS\_TOKEN\_API’:
    > "https://api.edx.org/oauth2/v1/access\_token",\
    > ‘EDXORG\_CLIENT\_CATALOG\_DETAIL\_API’:
    > "https://api.edx.org/enterprise/v1/enterprise-catalogs/”,\
    > ‘EDXORG\_COURSE\_DETAIL\_API’:"https://api.edx.org/enterprise/v1/enterprise-catalogs/",

3.  In lms/env/common.py,
    > Add ‘edxorg\_courses’ into installed apps\
    > Add “OPENEDX\_ROOT / 'features' / 'edxorg\_courses' /
    > 'templates',” in list of MAKO\_TEMPLATE\_DIRS\_BASE

4.  In lms/urls.py include app URLs:
    > url(r'', include('openedx.features.edxorg\_courses.urls')),

5.  In /lms/templates/discovery/course\_card.underscore change second
    > line as below\
    > &lt;% if (org == 'edX Courses') { %&gt;\
    > &lt;a href="/courses/&lt;%- course %&gt;/course\_about"&gt;\
    > &lt;% } else { %&gt;\
    > &lt;a href="/courses/&lt;%- course %&gt;/about"&gt;\
    > &lt;% } %&gt;

6.  Make LMS migrations
    > python manage.py lms makemigrations --settings=”production”\
    > python manage.py lms migrate --settings=”production”

7.  From LMS shell run below command for saving edxorg courses to database and reindex into ElasticSearch.
    > python manage.py lms set\_edxorg\_courses --settings=”production”
