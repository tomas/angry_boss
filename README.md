Angry Boss!
======

This is a simple script that takes care of managing pools of Delayed::Job workers dynamically.

Whenever there are pending jobs in queue, Angry Boss will fork new workers until the queue drops to zero, as long as the maximum limit of workers hasn't been reached (MAX_WORKERS).

This lets you save precious memory by not keeping X number of workers running all the time. When there are no jobs pending, Angry Boss will simply sit alone by himself.

How it works
--

Each worker is hired to perform a certain amount of jobs (JOBS_PER_BATCH). Once the worker is done, it will quit and then Angry Boss will spawn a new instance, in case there are pending jobs to be done.

This also means you don't need to worry about memory leaks as workers only live for a small period of time, similar to the way Resque handles workers.

Usage
--

Just copy the angry_boss script to your RAILS_ROOT/script directory. Make sure it has the executable bit and start it, passing the max number of workers you wish to be spawned. In other words:

    $ git clone git://github.com/tomas/angry_boss.git
    $ cp angry_boss/angry_boss /path/to/your/rails/app/script
    $ chmod +x /path/to/your/rails/app/script/angry_boss
    $ cd /path/to/your/rails/app
    $ RAILS_ENV=whatever script/angry_boss start 10

This will load your app's environment and start spawning workers until the count reaches 10, as long as there are pending jobs in queue. If you call Angry Boss with 0 workers, it will spawn a single worker for one loop, so you can test that your jobs are being processed correctly.

Monitoring
--

Angry Boss will leave an angry_boss.pid file in RAILS_ROOT/tmp/pids, so you can use God or Monit to make sure it keeps doing its job. As all workers descend from the master process, you can also monitor the total amount of memory used by all workers, so you can tune your settings to set the right limit on the maximum number of workers.

TODO
--

Add some tests. It works though. :)

Copyright
--

Written by Tomás Pollak.
Copyright (c) 2011 Fork Ltd. Licensed under the MIT license.
