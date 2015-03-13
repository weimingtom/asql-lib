Using of ASQL is very simple, very similar to URLLoader and other native classes. There are a few basic events available,
```
SQLEvent.CONNECT;// When connection is established

SQLEvent.SQL_ERROR;// Whenever any SQL error is received

SQLEvent.SQL_OK;// After every successfull INSERT query

SQLEvent.SQL_DATA;// After every successfull SELECT query.
```
Example:

```
package {
	import pl.mooska.asql.*;
	import pl.mooska.asql.events.*;
	import flash.display.Sprite;

	public class ASQL extends Sprite
	{
		private var connector:Asql = new Asql();

		public function ASQL()
		{
			connector.addEventListener( SQLEvent.CONNECT, handleConnect );
			
			connector.addEventListener( SQLError.SQL_ERROR, handleError );
			
			connector.addEventListener( SQLEvent.SQL_OK, handleOK );//Invoked after successfull INSERT
			
			connector.addEventListener(SQLEvent.SQL_DATA, handleData);//Invoked after successfull SELECT
			
			connector.connect("localhost", "root", "mooska", "flash" , 3306);//credentials, last arguments is mysql port, 3306 is default one, so you probably wont have to change it.
		}
		
		private function handleConnect ( evt:SQLEvent ) :void
		{
			trace("ASQL is connected");
         	connector.query("select * from book");
//Mysql query, dont have to be in any special form, just use it as you would on server side
		}
		private function handleError ( evt:SQLError ) :void
		{
			 trace("Error catched "+evt.text);
		}
		private function handleOK ( evt:SQLEvent ) :void
		{
			trace("Command was succesfull ");
		}
		private function handleData ( evt:SQLEvent ) :void
		{
			trace("Final data received");
			
			for(var i in evt.data )
			{
				trace( "value:"+i+" " +evt.data[i]);
			}

			
			//connector.disconnect();
		}
	}
}
```