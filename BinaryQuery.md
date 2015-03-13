To insert ByteArray into BLOB field of mysql table you should use binaryQuery method of Asql object.


Mysql table with BLOB field example:

```

CREATE TABLE `flash_bitmap` (
  `id` int(11) NOT NULL auto_increment,
  `bitmap` longblob NOT NULL,
  PRIMARY KEY  (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1 AUTO_INCREMENT=16 ;
```

binaryQuery example:
```
connector.binaryQuery( queryString, "wildcard", dataToSave );
//queryString:String, wildCard:String, data:ByteArray
//
//queryString should look like this: INSERT INTO `flash_bitmap` ( `id` , `bitmap` ) VALUES (NULL , wildcard)
//of course it can be as complicated as you want, this is just an example
//wildcard is a keyword ( you can set it in the second argument ) 
//It will be replaced with string representation of third arg, ByteArray
```

As you can see only one ByteArray/blob field at a time

Result comes as it did before, as an array of objects. Youll see, that when you want to insert some big ByteArray, players hangs for some time, its because of the convertion process, it iterates through whole array, so it can take some time.
Im working on making it faster.


How does it works and what does it mean ?
Blob field type, means, that you can keep in there absolutly any kind of data, ie. Bitmap, like in the example. Max file size is 16Mb.
If you can convert something to ByteArray, you can also place it in the dbase. Without serverside, or typical file system. Theres also compression available for this purpose (look into ByteArray methods) In this example, theres no native ByteArray compression, and image is not compressed like in jpg, all pixels go as they are.

One of the disadvantages is that, when you send data to the server, data length is twice as big as it should. It comes from string representation of ByteArray. Same thing you can notice in MIME coding. When selecting data from dbase, data goes with its original length.