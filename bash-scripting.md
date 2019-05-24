## Iterate through a tsv and do something for each line.

```
# $1 is the name of the file to consume.

echo $(wc -l $1)

while read item;
do
  project=$(echo $item | awk '{print $1}')
  sample=$(echo $item | awk '{print $2}')
  echo "Copying files for sample ${sample}, project ${project}"
done <$1
```

## Check the status of a script.

```
./script.sh
result=$?
if [[ $result -eq 1 ]]; then
  # Handle failure
else
  # Handle success
fi
```

## Capture output of command.

```
# The sed command removes trailing "/"
version=$(ls | tail -1 | awk '{print $2}' | sed 's/\/$//')

echo $version
```
