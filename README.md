libdigest
=========

Libdigest is a small C library for parsing and generating HTTP Digest Access Authentication ([rfc2617](https://www.ietf.org/rfc/rfc2617.txt)) header strings, both server side and client side.

Build it
--------

    $ make && make install

How to use it
-------------

### Client side

First, include the header files:

    #include <digest.h>
    #include <digest/client.h>

Create a new digest object with the value of the WWW-Authenticate header:

    digest_t d = digest_create("Digest realm=\"api\", qop=auth, nonce=dcd98b7102dd2f0e8b11d0f600bfb0c093");

Then supply the username, password and URI like below:

      digest_set_attr(d, D_ATTR_USERNAME, "jack");
      digest_set_attr(d, D_ATTR_PASSWORD, "Pass0rd");
      digest_set_attr(d, D_ATTR_URI, "/api/user");
      digest_set_attr(d, D_ATTR_METHOD, "POST");

To get the string to use in the Authorization header, call ´digest_get_hval()´, as below:

      char *v = digest_get_hval(d);

All the code (compile with ´-ldigest´):

    #include <digest.h>
    #include <digest/client.h>

    int main(int argc, char **argv)
    {
      char *digest_str = "Digest realm=\"api\", qop=auth, nonce=dcd98b7102dd2f0e8b11d0f600bfb0c093";
      printf("Generating header value for Authorization:\n%s\n", digest_str);

      digest_t d = digest_create(digest_str);
      digest_set_attr(d, D_ATTR_USERNAME, "jack");
      digest_set_attr(d, D_ATTR_PASSWORD, "Pass0rd");
      digest_set_attr(d, D_ATTR_URI, "/api/user");
      digest_set_attr(d, D_ATTR_METHOD, "POST");
      char *v = digest_get_hval(d);

      printf("Generated digest value:\n%s\n", v);

      return 0;
    }