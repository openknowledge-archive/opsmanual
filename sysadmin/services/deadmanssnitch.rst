HOWTO: "Dead man's switch" monitoring
=====================================

Monitoring things that are supposed to be working all the time (like a website)
is relatively easy to do. You simply check every few minutes that the website is
up, or that the server responds to pings.

Monitoring things that are supposed to happen periodically (like daily, weekly,
or monthly backups) is a little harder. One of the best ways to do this is a
`dead man's switch`_, also known as a dead man's handle. In a DMS system, an
external service expects to be notified every time the event occurs, and raises
the alarm if no report is received in a given time period.

.. _`dead man's switch`: https://en.wikipedia.org/wiki/Dead_man's_switch

There is a simple web service, `Dead Man's Snitch`_, which allows for easy
creation of an unlimited number of DMS-type monitoring checks for $19 per month.
We use this in a number of places to ensure that periodic events are working. In
the event that a task fails to report, Dead Man's Snitch will email our ticket
address, notifying us of the failure.

.. _`Dead Man's Snitch`: https://deadmanssnitch.com/

Creating a DMS monitoring check
-------------------------------

1.  Log in to https://deadmanssnitch.com/ using the account details stored in
    LastPass.
2.  Create a new "snitch," specifying the frequency at which the job should run.
    You will be given a URL that looks something like
    ``https://nosnch.in/5e5879c9be``. This is the URL that you have to request
    (usually programmatically) in order to report in for a given job.
3.  Make sure that the URL is called if and only if the job is successful. For
    example, here is a shell script that dumps PostgreSQL databases and calls
    DMS

    .. code-block:: sh

        #!/bin/sh

        # This ensures that if any command in the script exits abnormally (i.e.
        # with a non-zero exit code), the script will immediately abort.
        #
        # This is important, because it means that the request to Dead Man's
        # Snitch will not happen if pg_dump or s3cmd fails.
        set -e

        # It's also a good idea to detect and treat the use of undefined
        # environment variables as an error:
        set -u

        DUMPNAME="$(date +"%Y%m%d%H%M%S")-mydatabase.dump"

        # Dump the PostgreSQL database to a time
        pg_dump mydatabase >"$DUMPNAME"

        # Upload the dump to S3
        s3cmd put "$DUMPNAME" s3://backupbucket/postgres/

        # Record the file size
        DUMPSIZE=$(ls -lhd "$DUMPNAME" | awk '{ print $5 }')

        # If we got this far, all is well, and we should report to DMS. The
        # message part is completely optional, but it can be useful to return
        # basic stats (such as size of backup, or how long the task took), which
        # are then displayed on the DMS web interface.
        curl -sSL https://nosnch.in/5e5879c9be \
             -d "m=size: $DUMPSIZE" \
             >/dev/null





