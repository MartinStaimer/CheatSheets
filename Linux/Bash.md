
Dateien filtern nach Zeit:
```bash
find -mtime +{DAYS}
```

Dateien löschen nach Zeit:
```bash
find -mtime +{DAYS} -print0 | xargs -0 /bin/rm -f
```

Datum in Variable:
```bash
d=$(date +%Y-%m-%d-%H-%M-%S)
```

Prozess stoppen:
```bash
pkill -x {NAME_OF_PROCESS}
```

Parameter in Bash Skript:
Erstes Agrument  = $1, zweites = $2, usw.
```bash
if [ $1 == "{OPTION}"]
then
	do foo bar
fi
```

