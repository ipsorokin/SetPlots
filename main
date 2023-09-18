function Test-ReparsePoint([string]$Path) {
  $file = Get-Item $Path -Force -ea SilentlyContinue
  return [bool]($file.Attributes -band [IO.FileAttributes]::ReparsePoint)
}

# Main

[string]$ROOT_FOLDER = "C:\Plot\"
[object[]]$VOLUMES = Get-Volume | Where-Object {$_.FileSystemLabel -NE ""}
[string[]]$DIRS = (Get-ChildItem -Directory -Path $ROOT_FOLDER | Where-Object -FilterScript {(Test-ReparsePoint $_.FullName) -EQ $False}).FullName

foreach($vol in $VOLUMES) {
  $path = [IO.Path]::Combine($ROOT_FOLDER, $vol.FileSystemLabel)

  if ($DIRS.Contains($path)){
    $part = Get-Partition -Volume $vol
    Add-PartitionAccessPath -DiskNumber $part.DiskNumber -PartitionNumber $part.PartitionNumber -AccessPath $path
  }
}
