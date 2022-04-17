// TDX (Typed Data Exchange) format demo

```
ob [
    /*comment*/

    /*data types*/
    gi GuidProp = a84b9555-159d-4618-bde7-9c5c9fd1736d;
    
    st StringProp = "this is a string";
    in IntegerProp = 0;
    dc DecimalProp = 0.5;
    bo BooleanProp = true;
    hx HexadecimalProp = 0x1;
    sc ScriptProp = [
        "SCRIPT"
    ];
    ob ObjectProp = [
        st StringChildProp = "this is a child property"
    ];
    dy DynamicProp = st "enter any type here"; /* for dynamic value, the type should be in the value */
    dt DateTimeProp = 637857165117906400; /* stored in ticks */
    ts TimeSpanProp = 10000000; /* stored in ticks */
    dt DateOnlyProp = 01-01-2000; /* DD-MM-YYYY */
    sn ScientificNumber = 6.378571651179064e+17;

    // IDs
    id<gi> ModSQLIdProp = a84b9555-159d-4618-bde7-9c5c9fd1736d;
    id<in> IntegerIdProp = 283457;


    bi BinaryProp = [
        0000b50: 0400 0000 ffff 0900 0000 ffff 0900 0000 ................
        0000b60: ffff 0900 0000 ffff 0900 0000 ffff 0900 ............0..@
    ];

    //arrays
    ar<st> StringArrayProp = {
        "Item 1",
        "Item 2"
    };
    ar<in> IntArrayProp = {
        123414343,
        390239048
    };
    ar<dy> = {
        st "this is a string in a dynamic array",
        in 420,
        ar<st> {
            "array within a dynamic array"
        }
    };

    ar<ar<in>> ArrayOfIntArrays = {
        {
            2131209,
            1130912
        },
        {
            1321,
            2344,
        }
    }

    jn JsonProperty = [{
        "Count": 1756,
    }];

    xm XmlProperty = [
        <note>
            <to>Tove</to>
                <from>Jani</from>
                <heading>Reminder</heading>
            <body>Don't forget me this weekend!</body>
        </note>
    ];

    sq SQLProperty = [
        Iq = true;
        Tx = "select * from Users where Id=@IdParam and Name=@NameParam";
        Ps = [
            IdParam = "12198";
            NameParam = "John";
        ]
    ];

    /* todo */
    ex<in> ExpressionProperty = [
        rt GetJsonField("Title", JsonProperty);
    ];
]
```