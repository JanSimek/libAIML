libAIML
=======

Fork of Varol Okan's libAIML which itself is a fork of the original [C++ AIML parser from V01D](doc/markdown/history.md).

I also include the source of std_utils-0.7.5 in here to make it easier to compile the code

COMPILE AND RUN
---------------

You need [CMake](http://www.cmake.org) to build this program. 

```bash
mkdir build && cd build
cmake ..
make
```

### Troubleshooting

CMake sets wrong libxml2 include directory for me. If that happens to you too simply run ```cmake -DLIBXML2_INCLUDE_DIR=/usr/include/libxml2/ ..``` from the build directory.

JAVASCRIPT
----------

JavaScript is handled natively through [V8 JavaScript Engine](https://developers.google.com/v8/build). Please follow their instructions to build the libraries.

Since JS is not well specified in the AIML spec, I added [TestJavaScript.aiml](test_app/aiml/TestJavaScript.aiml) file to demonstrate how to use it with libAIML.

Note that the CDATA construct is required for libxml2 to properly read in the JS code.

Also please note that you will have access to the aiml - object in JavaScript. 

```JAVASCRIPT
  var aiml = { 
    user : "name",
    star : "star",
    that : "that",
    topic: "topic",
    bot  : {
      someBotVars ...
    }
  }
```

To test the JS categories you can enter:

```
You: test something
or
You: test aiml
```

```xml
<category>
  <pattern>TEST *</pattern>
  <template>
    This is cool <star/>.
    <javascript><![CDATA[
      print ( "   Testing JavaScript integration. Working !!! " + JSON.stringify ( aiml ) );
      var t;
      for ( var t=0; t<10; t++ )  {
        print ( "t=" + t );
      }
    ]]></javascript>
  </template>
</category>

<category>
  <pattern>TEST AIML</pattern>
  <template>
    <javascript><![CDATA[
      function objFields ( obj )  {
        for ( var key in obj )  {
         var val = obj[key];
         print ( "  Key[" +key+ "] = " + val );
        }
      }
      print ( "------------------ aiml - object ------------------" );
      objFields ( aiml );
      print ( "------------------ aiml.bot ------------------" );
      objFields ( aiml.bot );
    ]]></javascript>
  </template>
</category>
```

