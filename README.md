# My Snippets

```php
// Read a CSV File and convert it to an array
function csvFileToArray($filename, bool $useFirstLineAsKeys = false): array
{
    $list = [];
    $fileHandler = fopen($filename, 'r');
    if ($fileHandler === false) {
        throw new RuntimeException(sprintf('Could not open file %s', $filename));
    }

    $firstLine = true;
    $keys = [];
    while (($data = fgetcsv($fileHandler, 2000, ',', '"', '\\')) !== false) {
        if ($useFirstLineAsKeys && $firstLine) {
            $keys = $data;
            $firstLine = false;
            continue;
        }

        if ($useFirstLineAsKeys) {
            $tmpLine = [];
            foreach ($keys as $index => $keyValue) {
                $tmpLine[$keyValue] = $data[$index] ?? null;
            }
            $list[] = $tmpLine;
        } else {
            $list[] = $data;
        }
    }

    fclose($fileHandler);
    return $list;
}

function writeArrayToCsv($filename, array $content): void
{
    $fp = fopen($filename, 'w');
    foreach ($content as $fields) {
        fputcsv($fp, $fields, ',', '"', '');
    }
    fclose($fp);
}
```
