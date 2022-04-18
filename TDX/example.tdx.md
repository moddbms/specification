// TDX (Typed Data Exchange) format demo

```
ob [
    /*comment*/

    /*data types*/
    gid GuidProp = a84b9555-159d-4618-bde7-9c5c9fd1736d;
    
    str StringProp = "this is a string";
    int IntegerProp = 0;
    dec DecimalProp = 0.5;
    bol BooleanProp = true;
    hex HexadecimalProp = 0x1;
    scr ScriptProp = [
        "SCRIPT"
    ];
    obj ObjectProp = [
        st StringChildProp = "this is a child property"
    ];
    dyn DynamicProp = st "enter any type here"; /* for dynamic value, the type should be in the value */
    dtm DateTimeProp = 637857165117906400; /* stored in ticks */
    tsn TimeSpanProp = 10000000; /* stored in ticks */
    dto DateOnlyProp = 01-01-2000; /* DD-MM-YYYY */
    scn ScientificNumber = 6.378571651179064e+17;

    // IDs
    id<gid> ModSQLIdProp = a84b9555-159d-4618-bde7-9c5c9fd1736d;
    id<int> IntegerIdProp = 283457;


    bin BinaryProp = [
        0000b50: 0400 0000 ffff 0900 0000 ffff 0900 0000 ................
        0000b60: ffff 0900 0000 ffff 0900 0000 ffff 0900 ............0..@
    ];

    //arrays
    arr<str> StringArrayProp = {
        "Item 1",
        "Item 2"
    };
    arr<int> IntArrayProp = {
        123414343,
        390239048
    };
    arr<dyn> = {
        str "this is a string in a dynamic array",
        int 420,
        arr<str> {
            "array within a dynamic array"
        }
    };

    arr<arr<int>> ArrayOfIntArrays = {
        {
            2131209,
            1130912
        },
        {
            1321,
            2344,
        }
    }

    jsn JsonProperty = [{
        "Count": 1756,
    }];

    xml XmlProperty = [
        <note>
            <to>Tove</to>
                <from>Jani</from>
                <heading>Reminder</heading>
            <body>Don't forget me this weekend!</body>
        </note>
    ];

    sql SQLProperty = [
        IsQuery = true;
        Command = "select * from Users where Id=@IdParam and Name=@NameParam";
        Params = [
            IdParam = "12198";
            NameParam = "John";
        ]
    ];

    /* todo */
    exp<int> ExpressionProperty = [
        ret GetJsonField("Title", JsonProperty);
    ];
]
```