# FileManager 
The Filemanager class has some useful features which allows the reading and writing into editable files, transferring of files and zipping of files or directories.

#### Initializing class
This class can be initialized as shown below 

```php 
include_once "vendor/autoload.php";

use Spoova\FileManager\FileManager; 

$Filemanager = new FileManager;
```

#### Create new directory 
A new directory can only be created by supplying the full path of the new directory with the ```addDir()``` method. This method does not also update the last directory defined within the class. It only adds a new directory if it does not exist and returns true if the directory has been created or it already exists.

   ```php
   $Filemanager->addDir('some/new/dir'); //full path of new directory
   ```

#### Setting url for activity 
Aside from few methods like ```addDir()``` and ```openFile()``` methods which have stand-alone capabilities of overiding default urls, when using Filemanger for activities, a url or file path is expected to be defined which helps the Filemanager class to know where the operation is expected to be performed. A directory or file path can be specified with the ```setUrl()``` method. An example is shown below: 

   ```php 
   $Filemanager->setUrl(__DIR__); //example of setting a url 
   ```   

#### Create a new file 
To create a new file, we can use the ```openFile()``` method. This method will only create a file if the file does not exist. A boolean of true will be returned if the file created is readable.

   ```php
   $Filemanager->openFile($strict, $path); //full path of new directory
   ```

   + ```$strict``` a boolean value of true will allow creating a directory if the expected file destination does not already exist while false will prevent creating a new directory for a file if the directory does not already exist
   + ```$path``` an optional stand-alone file path for file to be created if not earlier defined with ```setUrl()```.

   ```php 
   $Filemanager->openFile(true, 'some/path/of/file.txt'); 
   ```

   > We can also specify the file path from the setUrl() method as shown below

   ```php
   $Filemanager->setUrl('some/path/of/file.txt');

   $Filemanager->openFile(true);
   ```

#### Create multiple files 
To create multiple files, we can use the ```openFiles()``` method. The first argument is an array list of full file paths of files expected to be created. The method will create directories for files if the directories do not exist and it only fails when an error is encountered and a file cannot be created. It returns true if all files were created successfully.

   ```php
   $Filemanager->openFiles($paths, $files);
   ```

   + ```$paths``` array list of file paths to be created
   + ```$files``` an optional variable that contains lists of successfully created file paths. 

   ```php 
   $Filemanager->openFiles(['file.txt', 'file2.txt'], $get_created_files); 
   ```



#### Working with files and directories
In order to work with a directory, the directory must be defined where an activity is expected to be performed using the ```setUrl()``` method. Once the source url 
is defined, we can proceed with the activity.

   ##### Set a url 
   ```php
   $Filemanager->setUrl(__DIR__); //specify directory
   ```

   ##### Get folders in specified directory 
   ```php
   $folders = $Filemanager->getFolders(); //full path of folders
   ```
   > Folder names only can be harvested without their full paths 
   ```php
   $folders = $Filemanager->getFolders(false); // names of folders
   ```   

   ##### Get files in specifed directory 
   ```php
   $files = $Filemanager->getFiles();
   ```

   > File names and extension can be harvested without their full paths as shown below
   ```php
   $files = $Filemanager->getFiles(false); // names of files
   ``` 

   > We can also get names of files without their extensions. However, it is important to always get extension names to know the type of file 
   ```php
   $files = $Filemanager->getFiles(false, false); // names of files without extension name
   ```   

   ##### Get contents of a directory 
   ```php
   $contents = $Filemanager->getContents(); //returns both files and folders
   ```

   ##### Zip a directory 
   To zip a url, the ```zipUrl()``` methods can be applied with the syntax below

   ```php
   $folders = $Filemanager->zipUrl($output_name, $excludes);
   ```

   + ```$output_name``` refers to the output name of the zip file. It uses default specified url if not defined.
   + ```$excludes``` refers to array list of file name in specified directory that should not be added to zipped file. 

   > We can check if a file is zipped by using the ```zipped()``` method. However, this will not tell if the zipped file is broken. 

   ```php 
   $Filemanager->setUrl(__DIR__);    
   $Filemanager->zipUrl(); 

   if($Filemanager->zipped()) {

        echo "Zip file created."

   } else {

        echo "Zip file not created!";

   }
   ```

   > We can get the last directory specified using the ```lastDir()``` method.

   ```php 
   $Filemanager->setUrl(__DIR__);    
   $Filemanager->zipUrl(); 

   if($Filemanager->zipped()) {

        echo $Filemanager->lastDir(); //directory of zipped file

   } else {

        echo "Zip file not created!";

   }
   ```

   ##### Copy from a directory to another
   To copy from last specified directory to another directory we can apply the ```copyTo()``` method. This will copy the entire folder to another specified directory 

   ```php
   $Filemanager->copyTo($newdir, $newname, $strict);
   ```

   + ```$newdir``` refers to the file or folder destination.
   + ```$newname``` refers to new name of file. This is optional
   + ```$strict``` boolean of true prevents any previous error from stopping operation.

   ```php
   $Filemanger->setUrl(__DIR__);

   $Filemanager->zipUrl();

   if($Filemanager->zipped()) {

        $Filemanager->copyTo('some/new/directory', 'zip_one'); //copy zip file to new directory

   }
   ```

   ##### Move last specified directory or file to another location
   To move the last specified directory to another location, the ```moveContentsTo()``` can be applied. This is mostly useful when we want to move a zipped file to another location.

   ```php
   $Filemanager->moveContentsTo($newdir, $ignore, $moved);
   ```

   + ```$newdir``` refers to the new file or folder destination.
   + ```$ignore``` refers to file or folder names within the last specified url to be ignored from being moved.
   + ```$moved``` this is used to store the names of moved files.

   An example of the above is shown below 

   ```php
   $Filemanger->setUrl(__DIR__);

   $Filemanager->zipUrl();

   if($Filemanager->zipped()) {

        $Filemanager->moveContentsTo('some/new/directory');

   }
   ```

   ##### Move last declared directory to another directory with a new name
   To move a last declared path to another directory while specifying a new output name, the ```moveTo()``` can be applied.

   ```php
   $Filemanager->moveTo($newdir, $name, $strict);
   ```

   + ```$newdir``` refers to new destination directory where last detected path will be moved into. 
   + ```$name``` Optional. Refers to new output name of a file if specified. This is useful when $newdir is a file path and not directory. If not specified, the moved file will maintain the original file name. 
   + ```$strict``` boolean of true prevents any previous error from stopping operation.

   An example of the above is shown below 

   ```php
   $Filemanger->setUrl(__DIR__);

   $Filemanager->zipUrl();

   if($Filemanager->zipped()) {

        $new_name = 'mydir.zip';
        
        $Filemanager->moveTo('some/new/directory', $new_name); //move zipped directory file to new path with new name. 

   }

   ```

   ##### Advanced moving of files and directories
   A more advanced method for moving files or directories to another is the ```move()``` method. The basic syntax is show below:
   ```php
   $Filemanager->move($param1, $param2);
   ```

   + ```$param1``` refers to new destination directory of selected file (or directory) or refers to the subpath of selected directory to be moved into destination directory $param2 if $param2 is defined. 
   + ```$param2``` refers to destination path of $param1 only if it is supplied.

   The example shown below is that in which a directory or file was moved to another directory. 

   ```php
   $Filemanger->setUrl(__DIR__.'/'.'folder_or_file_name'); //set a directory or file path
   
   $Filemanger->move(__DIR__.'/destination_directory'); //move to new destination directory

   ```
  
  > We can also specify that a directory within a primarily selected directory should be moved to another relative or absolute directory as shown below:

   ```php
   $Filemanger->setUrl(__DIR__); //set a base directory
   
   $Filemanger->move('folder_or_file', __DIR__.'/new_directory'); //move to new existing destination directory

   ```
  
  The ```move()``` method above will return true or false based on whether the file transfer was successful. 

   ##### Deleting a file or directory
   To delete a folder or file, the ```deleteFile()``` method can be employed
   ```php
   $Filemanager->deleteFile($dir);
   ```

   + ```$dir``` refers to the path of file or directory to be deleted. If not specified, it assumes the last specified directory.

   An example of the above is shown below 

   ```php
   $Filemanger->setUrl(__DIR__);

   $Filemanager->deleteFile(); //delete directory
   ```

   ##### Deleting a file only
   To delete a file only, the ```removeFile()``` method can be employed
   ```php
   $Filemanager->removeFile($dir, $check);
   ```

   + ```$dir``` refers to the path of file or directory to be deleted.
   + ```$check``` If set as true, will return true if specified file does not exist instead of false.

   An example of the above is shown below 

   ```php
   $Filemanger->setUrl(__DIR__);

   $Filemanager->deleteFile(); //delete directory
   ```

#### Extract from a zip file
   To extract a non-protected zipped file to a directory, the ```decompress()``` method can be applied.

   ```php
   $Filemanager->decompress($del, $strict);
   ```

   + ```$path``` refers to source path if ```$newPath``` is specified or destination path if ```$newPath``` is not specified.
   + ```$strict``` boolean of true prevents any previous error from stopping operation.

   An example of the above is shown below 

   ```php
   $Filemanger->setUrl(__DIR__);

   $Filemanager->zipUrl();

   if($Filemanager->zipped()) {

        $Filemanager->moveTo('some/new/directory'); //move zipped file to new path

   }

   $Filemanager->moveTo('some/old/path', 'some/new/path'); // move from some old to new path
   ```

#### Working with text readable files
Reading from files is usually done by specifying a key and a separator character. By default, Filemanager uses the colon ```:``` character to differentiate between keys and values. In some cases the default character is set as ```=``` as in the case of ```loadenv()``` method. We can read from a readable file by using the ```readFile()``` and ```readAll()``` method. 

   ##### Reading from text file 

   ```php
   $Filemanager->readFile($key, $separator);
   ```

   + ```$key``` name of key
   + ```$separator``` specified separator character. 

   > Assuming we have a file  _user.txt_ like below : 

   ```txt
   name:  Foo name; 
   class: Foo class;
   ```

   > we can get the contents of the text file using the format below 

   ```php 
   $Filemanager->setUrl('user.txt');

   $contents = $Filemanager->readFile(['name'], ":"); // ['name' => 'Foo name']
   ```
   
   > By default, the delimiter ```;``` is usually removed whether specified or not. We can also read all contents with ```readAll()``` as shown below : 

   ```php
   $Filemanager->setUrl('user.txt');

   $contents = $Filemanager->readFile(['name', 'class']); // ['name' => 'Foo name', 'class'=> 'Foo class']   
   ```
   
   > If a key that does not exist is being searched, the returned value will be an empty string

   ```php
   $Filemanager->setUrl('user.txt');

   $contents = $Filemanager->readFile(['user'], ":"); // ['user' => '']   
   ```

   ##### Editing a text file 
   There are five different ways of modifying text files which include the use of ```textLine()```, ```textWrite()```, ```textUpdate()```, ```textReplace()``` and ```textDelete()``` methods.

   ###### textLine()
   This method is used to write a new line into text file with options before or after a specified line.

   ```php
   $Filemanager->textLine($number, $position);
   ```

   + ```$number``` number of new lines to be added to text file.
   + ```$position``` specifies where the lines should be added. 

   > Assuming we have a file  _user.txt_, we can add new lines to the file using the format below 

   ```php 
   $Filemanager->setUrl('user.txt');
    
   $contents = $Filemanager->textLine(2); // add two new lines
   ```
   
   > If the file already contains a text, we can specify the position of line using ```after``` or ```before``` as shown below : 

   ```txt
   name:  Foo name; 
   class: Foo class;
   ```
     
   ```php
   $Filemanager->setUrl('user.txt');

   $Filemanager->textLine(2, ['after' => 'name']); // add two lines after line of key 'name:'
   ```
       
   ###### textWrite()
   This method is used to write texts into a text file
  
   ```php
   $Filemanager->textLine($data, $options);
   ```

   + ```$data``` array list of keys and values pairs
   + ```$options``` used to define position and separator character 

   > Assuming we have a file  _user.txt_, we can add new keys to the file using the format below :

   ```php 
   $Filemanager->setUrl('user.txt');
    
   $Filemanager->textWrite(['name' => 'foo']); // add two new lines
   ```
   
   > We can specify separator character through supplied options with ```separator``` key. Also if the file already contain keys, we can specify the position of new keys  with ```before``` or ```after``` options as shown below : 

   ```txt
   name:  Foo name; 
   class: Foo class;
   ```    

   ```php 
   $Filemanager->setUrl('user.txt');    

   $Filemanager->textWrite(['age' => 'foo'], ['after' => 'name', 'separator' => ':']); // add key after "name"
   ```

   ###### textUpdate()
   This method is used to update the values of already existing keys in a text file. If the specified keys do not exist, then, the keys along with their values will be added on new lines
  
   ```php
   $Filemanager->textUpdate($data, $upds, $separator);
   ```

   + ```$data``` array list of keys and values pairs
   + ```$upds``` used to store only updated values
   + ```$separator``` used to set separator character. Default is ```colon``` sign. 

   > Assuming we have a file  _user.txt_, we can add or update keys of the file using the format below :

   ```txt
   name:  Foo name; 
   class: Foo class;
   ```    

   ```php 
   $Filemanager->setUrl('user.txt');
    
   $Filemanager->textUpdate(['name' => 'Bar name', 'age' => '50']); // 
   ```
   
   > The resulting text file will look like the format below

   ```txt
   name:  Bar name; 
   class: Foo class;
   age: 50;
   ```   

   ###### textReplace()
   This method is used to replace a key's value only if that key exists.
  
   ```php
   $Filemanager->textReplace($data, $separator);
   ```

   + ```$data``` array list of keys and values pairs
   + ```$separator``` used to set separator character. Default is ```colon```

   > Assuming we have a file  _user.txt_, we can replace the existing keys' value :

   ```txt
   name:  Foo name; 
   class: Foo class;
   ```    

   ```php 
   $Filemanager->setUrl('user.txt');
    
   $Filemanager->textUpdate(['name' => 'Bar name']); // 
   ```
   
   > The resulting text file will look like the format below

   ```txt
   name:  Bar name; 
   class: Foo class;
   ```   

   ##### Reading a file without instantiating the class 
   We can read a readable file without instantiating the class itself by using the ```load()``` method. This method will read all the contents of a file which is equivalent to the ```readAll()``` method. 

   ```php
   Filemanager::load($url, $separator); 
   ```

   + ```$url``` path of the file to be read
   + ```$separator``` a key to value separator character 

   ##### Reading a file into the global ENV environment 
   Readable file keys and value can be stored into the global ```$_ENV``` environment using the ```loadenv()``` method. By default, values are stored under the ```$_ENV[':ENV']``` key. This method is mainly used to read from ```.env``` files. 

   ```php
   Filemanager::loadenv($url, $key, $separator); 
   ```

   + ```$url``` path of the file to be read
   + ```$key``` default key is _':ENV'_ . While boolean value of true will store data directly to ```$_ENV```, false prevents overwriting predefined default $_ENV key. If a custom string name is defined, the name will be used as replacement for the default ':ENV'.
   + ```$separator``` a key to value separator character. Note that the default character for this method is set as equal sign rather than semicolon. 

   ##### Detecting ENV key
   The ```env_key()``` method is used to detect the last custom key used in ```loadenv()``` method. By default this returns ```:ENV```.

   ##### Detecting ENV data
   The ```env_data()``` method is used to detect the last data saved into the ```$_ENV``` global variable when ```loadenv()``` method was used. 

#### Fetching errors 
The method ```err()``` returns the last error detected if an error occurs within the Filemanager system. However, when working with zip files, the ```fails()``` and ```succeeds()``` method can help to detect the error that occurs in the process of zipping or decompressing a file. Example of usage is shown below 

```php 
$Filemanager->setUrl('some/dir'); 

$Filemanager->zipUrl(); 

if($Filemanager->succeeds()) {

     echo 'Zipping successful';

}elseif($Filemanager->fails()) {

     echo $Filemanager->error();  

} else {
     
     echo 'Something is wrong!';

}

