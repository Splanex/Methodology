	ffuf -request [request file] -request-proto http -w [wordlist] -fs [size to filter out]

	ffuf -request [request file] -request-proto http -mode clusterbomb -w [pass wordlist]:FUZZPASS -w [user wordlist]:FUZZUSER -fs [size to filter out]

	hydra -u [user] -P [password fike] [ip] http-post-form "[path]:[data]:[error message]"