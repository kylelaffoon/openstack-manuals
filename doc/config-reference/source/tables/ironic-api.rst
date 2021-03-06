..
    Warning: Do not edit this file. It is automatically generated from the
    software project's code and your changes will be overwritten.

    The tool to generate this file lives in openstack-doc-tools repository.

    Please make any changes needed in the code, then run the
    autogenerate-config-doc tool from the openstack-doc-tools repository, or
    ask for help on the documentation mailing list, IRC channel or meeting.

.. _ironic-api:

.. list-table:: Description of API configuration options
   :header-rows: 1
   :class: config-ref-table

   * - Configuration option = Default value
     - Description
   * - **[api]**
     -
   * - ``api_workers`` = ``None``
     - (Integer) Number of workers for OpenStack Ironic API service. The default is equal to the number of CPUs available if that can be determined, else a default worker count of 1 is returned.
   * - ``enable_ssl_api`` = ``False``
     - (Boolean) Enable the integrated stand-alone API to service requests via HTTPS instead of HTTP. If there is a front-end service performing HTTPS offloading from the service, this option should be False; note, you will want to change public API endpoint to represent SSL termination URL with 'public_endpoint' option.
   * - ``host_ip`` = ``0.0.0.0``
     - (String) The IP address on which ironic-api listens.
   * - ``max_limit`` = ``1000``
     - (Integer) The maximum number of items returned in a single response from a collection resource.
   * - ``port`` = ``6385``
     - (Unknown) The TCP port on which ironic-api listens.
   * - ``public_endpoint`` = ``None``
     - (String) Public URL to use when building the links to the API resources (for example, "https://ironic.rocks:6384"). If None the links will be built using the request's host URL. If the API is operating behind a proxy, you will want to change this to represent the proxy's URL. Defaults to None.
   * - **[cors]**
     -
   * - ``allow_credentials`` = ``True``
     - (Boolean) Indicate that the actual request can include user credentials
   * - ``allow_headers`` = ``Content-Type, Cache-Control, Content-Language, Expires, Last-Modified, Pragma``
     - (List) Indicate which header field names may be used during the actual request.
   * - ``allow_methods`` = ``GET, POST, PUT, DELETE, OPTIONS``
     - (List) Indicate which methods can be used during the actual request.
   * - ``allowed_origin`` = ``None``
     - (List) Indicate whether this resource may be shared with the domain received in the requests "origin" header.
   * - ``expose_headers`` = ``Content-Type, Cache-Control, Content-Language, Expires, Last-Modified, Pragma``
     - (List) Indicate which headers are safe to expose to the API. Defaults to HTTP Simple Headers.
   * - ``max_age`` = ``3600``
     - (Integer) Maximum cache age of CORS preflight requests.
   * - **[cors.subdomain]**
     -
   * - ``allow_credentials`` = ``True``
     - (Boolean) Indicate that the actual request can include user credentials
   * - ``allow_headers`` = ``Content-Type, Cache-Control, Content-Language, Expires, Last-Modified, Pragma``
     - (List) Indicate which header field names may be used during the actual request.
   * - ``allow_methods`` = ``GET, POST, PUT, DELETE, OPTIONS``
     - (List) Indicate which methods can be used during the actual request.
   * - ``allowed_origin`` = ``None``
     - (List) Indicate whether this resource may be shared with the domain received in the requests "origin" header.
   * - ``expose_headers`` = ``Content-Type, Cache-Control, Content-Language, Expires, Last-Modified, Pragma``
     - (List) Indicate which headers are safe to expose to the API. Defaults to HTTP Simple Headers.
   * - ``max_age`` = ``3600``
     - (Integer) Maximum cache age of CORS preflight requests.
   * - **[oslo_middleware]**
     -
   * - ``max_request_body_size`` = ``114688``
     - (Integer) The maximum body size for each request, in bytes.
   * - ``secure_proxy_ssl_header`` = ``X-Forwarded-Proto``
     - (String) DEPRECATED: The HTTP Header that will be used to determine what the original request protocol scheme was, even if it was hidden by an SSL termination proxy.
   * - **[oslo_versionedobjects]**
     -
   * - ``fatal_exception_format_errors`` = ``False``
     - (Boolean) Make exception message format errors fatal
