# tar

### Create tar-file under Windows

- Use ````SharpZipLib.NETStandard```` from ````ICSharpCode````
- Snippet that creates an archive: 

````
Directory.SetCurrentDirectory(@"C:\Temp\MyFiles\");
string[] files = { "A.txt" , "B.txt" , "C.txt" };
 
using (FileStream outStream = File.Create(@"C:\Temp\archive.tar"))
{
    TarArchive archive = TarArchive.CreateOutputTarArchive(outStream);

    foreach (string file in files)
    {
        TarEntry entry = TarEntry.CreateEntryFromFile(file);
        archive.WriteEntry(entry, true);
    }

    archive.Close();
}
````

---

#### Check tar under Linux
- Check file-type under linux: ````file archive.tar````  (should return ````POSIX tar archive````)
- List files in tar: ````tar -tvf archive.tar````
- Extract files in tar: ````tar -xvf archive.tar````
