# Ampache Music Server

Private fork of [jgoerzen/ampache](https://salsa.debian.org/jgoerzen/docker-ampache-base) that tries to stay up to date with with the current ampache version.
   
This image provides the Ampache server, with full support for transcoding
on the fly.

You can download with:

    docker pull jgoerzen/ampache-mysql

And run with something like this:

    docker run -td -p 8080:80 -p 80443:443 \
    --stop-signal=SIGRTMIN+3 \ 
    --tmpfs /run:size=100M --tmpfs /run/lock:size=100M \
    -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
    -v /musicdir:/music:ro \
    -v /playlistdir:/playlists:rw \
    --name=ampache jasminemoeller/ampache

Or with a newer systemd, as in Debian bullseye on the host

    docker run -td -p 8080:80 -p 80443:443 \
    --stop-signal=SIGRTMIN+3 \ 
    --tmpfs /run:size=100M --tmpfs /run/lock:size=100M \
    -v /sys/fs/cgroup:/sys/fs/cgroup:rw --cgroupns=host \
    -v /musicdir:/music:ro \
    -v /playlistdir:/playlists:rw \
    --name=ampache jasminemoeller/ampache

This will expose your music stored at `/musicdir` on the host in read-only mode, and your playlists
stored at `/playlistdir` in read-write mode, to the container.  You will probably also
want to add a `-v` in some fashion covering `/var/www/html/ampache/config`, since that you will want
to preserve those files as well.

# Setup

Now, point a browser at http://localhost:8080/ampache and follow the
on-screen steps, using the [Ampache install docs](https://github.com/ampache/ampache/wiki/Installation)
as a guide.

Once configured, add a catalog pointing to `/music` at <http://localhost:8080/ampache/index.php#admin/catalog.php?action=show_add_catalog>, and another for `/playlists`.

# Ports

By default, this image exposes a HTTP server on port 80, HTTPS on port 443, and
also exposes port 81 in case you wish to use it separately for certbot or another
Letsencrypt validation system.  HTTPS will require additional configuration.

Ampache is exposed at path `/ampache` on the configured system. 

# Source

This is prepared by John Goerzen <jgoerzen@complete.org> and the source
can be found at https://github.com/jgoerzen/docker-ampache

# Copyright

Docker scripts, etc. are
Copyright (c) 2017-2022 John Goerzen
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions
are met:
1. Redistributions of source code must retain the above copyright
   notice, this list of conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright
   notice, this list of conditions and the following disclaimer in the
   documentation and/or other materials provided with the distribution.
3. Neither the name of the University nor the names of its contributors
   may be used to endorse or promote products derived from this software
   without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE AUTHORS AND CONTRIBUTORS ``AS IS'' AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED.  IN NO EVENT SHALL THE REGENTS OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
SUCH DAMAGE.

Additional software copyrights as noted.

