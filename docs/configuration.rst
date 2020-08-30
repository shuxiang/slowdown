=============
Configuration
=============

.. contents::
    :depth: 1
    :local:
    :backlinks: none


The config file of the slowdown server called `slowdown.conf` is placed in
the `etc` folder. The following is a detailed config example of all the
sections and options.

.. code-block:: apacheconf

    # file: etc/slowdown.conf

    # Effective User
    # The default is the current user.
    #
    #user nobody

    # Process Name
    #
    #proc slowdown

    # Log Level
    #
    # Set log level to logging.DEBUG:
    #verbose 2
    # Set log level to logging.INFO
    #verbose 1
    # Quiet mode (default):
    verbose 0

    <resource>

        # Limits for open files
        #
        RLIMIT_NOFILE 65535

    </resource>

    <environment>

        # By default, DummyThreadPool is used, which is a threadpool that
        # does not actually use threads and blocks the entrie program.
        #
        #GEVENT_THREADPOOL slowdown.threadpool.DummyThreadPool

        # Other runtime environment
        #
        #ENV value

    </environment>

    # URL Routing based on regular expression.
    <routers>
        <router DEFAULT>

            # A regular expression to match hosts
            # Group name must be uppercased
            #
            pattern ^(?P<ALL_HOSTS>.*)$$

            <host ALL_HOSTS>

                # A reqular expression to match PATH_INFO and set
                # rw.environ['locals.path_info'] to the named group.
                # Group name must be uppercased.
                #
                pattern ^/mysite(?P<MYSITE>/.*)$$

                <path MYSITE>
                    # The package called 'mysite' placed in
                    # the 'pkgs/' dir is set to handle
                    # incoming requests.
                    #
                    handler mysite
                </path>

                # Another rule
                #
                pattern ^(?P<ITWORKS>/.*)$$

                <path ITWORKS>
                    # It works!
                    #
                    # A handler comes from
                    # the slowdown package.
                    #
                    handler slowdown.__main__
                </path>
            </host>

            # More hosts ..
            #
            #<host HOSTNAME>...</host>

        </router>

        # More routers
        #
        #<router>...</router>

    </routers>

    <servers>
        <http MY_HTTP_SERVER>
            address  0.0.0.0:8080
            address  127.0.0.1:9080

            # More addresses
            #
            #address host:port

            router   DEFAULT
        </http>
        <https MY_HTTPS_SERVER>
            address  0.0.0.0:8443
            address  127.0.0.1:9443

            # More addresses
            #
            #address host:port

            router   DEFAULT
            keyfile  /PATH/TO/server.key
            certfile /PATH/TO/server.cert
        </https>

        # More servers
        #
        #<http>...</http>
        #<https>...</https>

    </servers>

    # Run scripts at startup
    <scripts>
        # Run a module or package with `main` function
        #
        run script

        # More scripts
        #
        #run ..
    </scripts>

.. note::

    Section names, regex group names, option names, must be written in
    uppercase because `ZConfig`_ is case-insensitive. See `ZConfig`_ for
    details.

.. note::

    `$` must escape to `$$` in patterns because `$` is used to define
    variables. See `ZConfig`_ for details.

.. _ZConfig: https://zconfig.readthedocs.io/en/latest/