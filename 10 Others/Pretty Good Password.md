	gpg2john [file].asc > privatejohn
	john privatejohn

	gpg --import [file].asc
	gpg --decrypt [file].pgp