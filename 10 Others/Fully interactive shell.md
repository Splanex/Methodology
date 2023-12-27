## Attacker
	echo $TERM
	stty -a

## Victim
	export SHELL=bash
	export TERM=$x
	stty rows $x columns $y