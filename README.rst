``logparse``
============

This module implements a simple but somewhat configurable parser
for apache HTTPd log files.

None of the other modules I found correctly handled escaped chars
in quoted strings.

A parser for the combined log format is predefined at
``logparse.combined_format``. It should be straightforward to define
other formats without much trouble (see how combined_format is
created for reference).

Example::

    $ python
    In [1]: import logparse

    In [2]: line = r'127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326 "http://www.example.com/start.html" "User agent\nwith some random escaped characters: \"\xe2\x98\x83\""'

    In [3]: print line
    127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326 "http://www.example.com/start.html" "User agent\nwith some random escaped characters: \"\xe2\x98\x83\""

    In [4]: logparse.combined_format.parse(line)
    Out[4]:
    {'datetime': datetime.datetime(2000, 10, 10, 20, 55, 36, tzinfo=<UTC>),
     'referer': 'http://www.example.com/start.html',
     'remote_host': '127.0.0.1',
     'remote_logname': '-',
     'remote_user': 'frank',
     'request': ('GET', '/apache_pb.gif', 'HTTP/1.0'),
     'response_size': 2326,
     'status': 200,
     'user_agent': 'User agent\nwith some random escaped characters: "\xe2\x98\x83"'}

    In [5]: print _["user_agent"]
    User agent
    with some random escaped characters: "☃"
