---
layout: post
title: "[ ASP ] Classic ASP JSON형태로 데이터가져와서 1년달력만들기"
subtitle: "etc"
categories: back
comments: true
---

기술블로그를 하면서 느끼는점은, 개발자들은 물론 구글의 도움도 많이 받지만 틀린정보가 있다고해서 귀찮아한다거나

왜 제대로 된 정보가 없냐며 탓을 할 문제는 전혀 아니라는것이다. 왜냐면, 물론 맞는 정보면 더할나위 없이 좋겠지만

보통은 본인의 기억을 정리하기 위해 적어놓는것이고 그런 글들도 직접 코딩해보고 테스트해봐야하는 것들은 순전히

개발자의 몫이기 때문이다.

이 말을 하는건 내가 그랬기 때문이닼ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ... asp정보를 찾고싶은데 온갖거로

테스트해봤는데 다 틀린정보였어서 짜증이 났지만 그걸 짜증내서 뭐하며 그냥 힌트만 얻는걸로 족해야하지 않겠나?

그 모든 힌트를 조합하니 결국에는 에러없이 실행되었고 그거면 난 됐다..!!!

하지만 나같이 삽질하는 사람이 조금이라도 시간을 아끼길 바라면서..흑흑

일을 하면서 정기적으로 매년마다 처리해야하는 일들이 일정하게 있는 편들이라 (예를들어 4월에는 뭐, 6월에는 뭐, 7월애는 뭐)

1년일정을 한눈에 볼 수 있는 그래프를 만들고싶었다.

가장 쉽게 생각할수있는 디자인은 깃허브의 컨트리뷰션 그래프같은것!

깃헙그래프를 보면 한눈에 일년의 과정들이 눈에 들어오고 그 내용을 보려면 그 칸을 클릭해보면 되기에

일정관리하기에 편할것 같다는 생각이 들어서 만들어보려고 했다.

뭐 결과물은 아래처럼...(있어보이게 블러처리하고 싶었지만 귀차나씀)

![](/assets/img/calendar.PNG)

기능은

1. 저 색깔box를 클릭하면 상세정보를 보여주고
2. 저 box가 아닌곳에 mouseover가 되면 +가 되면서 일정을 추가할수있게 한다.
3. 1년달력으로 날짜가 유효하지 않은곳은 빗금을 쳐서 표시가 되게 한다.(ex.2월30일)
4. 윤년체크한다.

우선은 내가 사용하고 있는 스택은 classic ASP이고, 서버사이드스크립트이다. DB와 보통 연결해서 바로 가져오는데

이번에는 한번 JSON으로 받아오기로 함

아무래도 JSON으로 받아오면 데이터의 이동만 있기때문에 Ajax를 사용할때 좀더 편리하기 때문에

frame을 더이상 사용하지 않을 수 있겠다는 생각에 지금 프로젝트를 시작하게 되었다.

우선 ASP에서 json을 지원해주고 있지는 않지만 이것저것 찾아보니 누가 만들어놓은 클래스같은 것들이 꽤나 있었다.

그런데 다들 너무 중구난방임.

막상 다 가져가서 똑같이해도 미친듯이 에러뿜는 ASP여......

한 500번은 try해본것 같다.

우선 그러니까 필요한 라이브러리asp에는

1. JSON.asp
2. json.objectClass.asp
3. aspJSON.asp

이 3가지가 있다.

1번의 경우에는 new JSON으로 인스턴트를 만들때 사용된다.

2번의 경우에는 new JSONobject를 만들고 parsing을 해주는데 필요하다

3번은 DB로부터 가져온 데이터들을 for문 돌릴때 필요하다.

저 위의 파일은 아래에다가 코드로 첨부해야지(뭔가 첨부파일로 첨부하면 다운받기 싫음)

굉장히 많은 asp파일들이 떠다닌다. json2.asp / json2.js / json2.min.js 어쩌구저쩌구 그걸 활용해볼수도 있겠지만

내 경우에는 아무것도 안됐었고 에러만 뿜어댐

되는건 저 3가지 짬뽕해야지만 에러없이 데이터를 가져올 수 있었다.

우선 내가 웹서버에 뿌리는 코드는 calendar.asp인데

```
Response.LCID = 1046
```

이게 코드를 추가하는게 중요한것 같았다(타임설정해주는 건데 왜 중요한건지 모르겠지만 여튼 저거 없으면 에러뿜음).

json.objectClass.asp인클루드하고 LCID없으니까 에러뿜던데..

asp로 가져오긴 했지만 최대한 asp를 안쓰고 js로 만들어보려고했다.

기본적으로 ajax로 데이터를 가져오면 js로 가져오는거기 때문에 그 뒤로는 최대한 js로 가지고 놀려고 했고,

이걸 하다보니까 또 생각난게 이럴거면 프레임웤으로 해야봐야게따..라는 생각이 들면서

다시 node.js와 vue 강의를 듣고있다는 사실이다. (욕심이 너무 많음)

여튼, 더 자세한 사항은 코드 뜯어서 분석해보길...

이정도 적어놓으면 나중에 내 기억이 기억할거라 믿으면서...(과연)

calendar.asp가 로딩되면 script가 실행되면서 callingajax.asp를 호출한다.

그 callingajax에서 db내용을 json으로 바꿔주고 그 바꿔진 json데이터를 ajax success안으로 가져오는것임.

### calendar.asp

```asp
<body topmargin="10" leftmargin="15" onload="fetchJson()">
```

### calendar.asp의 script

```javascript
function fetchJson() {
  var k = 0;
  var allData = { k: k };

  $.ajax({
    type: "POST",
    url: "callingajax.asp",
    data: allData,
    dataType: "json",

    success: function (result) {
      console.log(result.datalist);
      for (i = 1; i < result.datalist.length; i++) {
        gubun = result.datalist[i].gubun;
        subject = result.datalist[i].subject;
        contents = result.datalist[i].contents;
        date1 = result.datalist[i].date1;
        thisyear = result.datalist[i].thisyear;
        ryn = result.datalist[i].ryn;
        url = result.datalist[i].url;

        console.log("subject" + subject);
      }
    },
    error: function () {
      alert("failed to fetch json data!");
    },
  });
}
```

### callingajax.asp

```asp
<%
Response.CharSet="euc-kr"
Session.codepage="949"
Response.codepage="949"
Response.ContentType="text/html;charset=euc-kr"
%>
<%
Response.LCID = 1046
%>
<!--#includes file="../../include/dbconnect.asp"-->

<!--#includes file="include/JSON.asp" -->
<!--#includes file="include/json.objectClass.asp"-->
<!--#includes file="include/aspJSON.asp" -->
<%
cust_id = Session("cust_id")
admin_id = Session("admin_id")


Set List = Server.CreateObject("ADODB.Recordset")
sqlview = "select * from calendar order by date1"

List.Open sqlview, dbCon, 1
ListCount = List.RecordCount

Set caljson = new JSON  'JSON.asp가 필요함
Set rs = dbCon.Execute(sqlview)

Dim List_jsonString
List_jsonString = caljson.toJSON("datalist", rs, false)

Dim jsonObj, outputObj
set jsonObj = new JSONobject
set outputObj = jsonObj.parse(List_jsonString)  'json.objectClass.asp가 필요함(파싱하려면)


Set oJSON = New aspJSON     'aspJSON.asp 필요함(for문)
oJSON.loadJSON(List_jsonString)

Response.write List_jsonString
%>
```

---

### JSON.ASP

```asp
<%
'**************************************************************************************************************

'' @CLASSTITLE:     JSON
'' @CREATOR:        Michal Gabrukiewicz (gabru at grafix.at), Michael Rebec
'' @CONTRIBUTORS:   - Cliff Pruitt (opensource at crayoncowboy.com)
''                  - Sylvain Lafontaine
''                  - Jef Housein
''                  - Jeremy Brown
'' @CREATEDON:      2007-04-26 12:46
'' @CDESCRIPTION:   Comes up with functionality for JSON (http://json.org) to use within ASP.
''                  Correct escaping of characters, generating JSON Grammer out of ASP datatypes and structures
''                  Some examples (all use the <em>toJSON()</em> method but as it is the class' default method it can be left out):
''                  <code>
''                  <%
''                  'simple number
''                  output = (new JSON)("myNum", 2, false)
''                  'generates {"myNum": 2}
''
''                  'array with different datatypes
''                  output = (new JSON)("anArray", array(2, "x", null), true)
''                  'generates "anArray": [2, "x", null]
''                  '(note: the last parameter was true, thus no surrounding brackets in the result)
''                  % >
''                  </code>
'' @REQUIRES:       -
'' @OPTIONEXPLICIT: yes
'' @VERSION:        1.5.1

'**************************************************************************************************************
class JSON

    'private members
    private output, innerCall

    'public members
    public toResponse       ''[bool] should the generated representation be written directly to the response (using <em>response.write</em>)? default = false
    public recordsetPaging  ''[bool] indicates if only the current page should be processed on paged recordsets.
                            ''e.g. would return only 10 records if <em>RS.pagesize</em> is set to 10. default = false (means that always all records are processed)

    '**********************************************************************************************************
    '* constructor
    '**********************************************************************************************************
    public sub class_initialize()
        newGeneration()
        toResponse = false
        recordsetPaging = false
    end sub

    '******************************************************************************************
    '' @SDESCRIPTION:   STATIC! takes a given string and makes it JSON valid
    '' @DESCRIPTION:    all characters which needs to be escaped are beeing replaced by their
    ''                  unicode representation according to the
    ''                  RFC4627#2.5 - http://www.ietf.org/rfc/rfc4627.txt?number=4627
    '' @PARAM:          val [string]: value which should be escaped
    '' @RETURN:         [string] JSON valid string
    '******************************************************************************************
    public function escape(val)
        dim cDoubleQuote, cRevSolidus, cSolidus
        cDoubleQuote = &h22
        cRevSolidus = &h5C
        cSolidus = &h2F
        dim i, currentDigit
        for i = 1 to (len(val))
            currentDigit = mid(val, i, 1)
            if ascw(currentDigit) > &h00 and ascw(currentDigit) < &h1F then
                currentDigit = escapequence(currentDigit)
            elseif ascw(currentDigit) >= &hC280 and ascw(currentDigit) <= &hC2BF then
                currentDigit = "\u00" + right(padLeft(hex(ascw(currentDigit) - &hC200), 2, 0), 2)
            elseif ascw(currentDigit) >= &hC380 and ascw(currentDigit) <= &hC3BF then
                currentDigit = "\u00" + right(padLeft(hex(ascw(currentDigit) - &hC2C0), 2, 0), 2)
            else
                select case ascw(currentDigit)
                    case cDoubleQuote: currentDigit = escapequence(currentDigit)
                    case cRevSolidus: currentDigit = escapequence(currentDigit)
                    case cSolidus: currentDigit = escapequence(currentDigit)
                end select
            end if
            escape = escape & currentDigit
        next
    end function

    '******************************************************************************************************************
    '' @SDESCRIPTION:   generates a representation of a name value pair in JSON grammer
    '' @DESCRIPTION:    It generates a name value pair which is represented as <em>{"name": value}</em> in JSON.
    ''                  the generation is fully recursive. Thus the value can also be a complex datatype (array in dictionary, etc.) e.g.
    ''                  <code>
    ''                  <%
    ''                  set j = new JSON
    ''                  j.toJSON "n", array(RS, dict, false), false
    ''                  j.toJSON "n", array(array(), 2, true), false
    ''                  % >
    ''                  </code>
    '' @PARAM:          name [string]: name of the value (accessible with javascript afterwards). leave empty to get just the value
    '' @PARAM:          val [variant], [int], [float], [array], [object], [dictionary], [recordset]: value which needs
    ''                  to be generated. Conversation of the data types is as follows:<br>
    ''                  - <strong>ASP datatype -> JavaScript datatype</strong>
    ''                  - NOTHING, NULL -> null
    ''                  - INT, DOUBLE -> number
    ''                  - STRING -> string
    ''                  - BOOLEAN -> bool
    ''                  - ARRAY -> array
    ''                  - DICTIONARY -> Represents it as name value pairs. Each key is accessible as property afterwards. json will look like <code>"name": {"key1": "some value", "key2": "other value"}</code>
    ''                  - <em>multidimensional array</em> -> Generates a 1-dimensional array (flat) with all values of the multidimensional array
    ''                  - RECORDSET -> array where each row of the recordset represents a field of the array. Each array field has properties according to the column names of the recordset (<strong>lowercase!</strong>) e.g <em>toJSON("r", RS, false)</em> can be accessed afterwards with <em>r[0].id</em>
    ''                  - <em>request</em> object -> every property and collection (cookies, form, querystring, etc) of the asp request object is exposed as an item of a dictionary. Property names are <strong>lowercase</strong>. e.g. <em>servervariables</em>.
    ''                  - OBJECT -> name of the type (if unknown type) or all its properties (if class implements <em>reflect()</em> method)
    ''                  Implement a <strong>reflect()</strong> function if you want your custom classes to be recognized. The function must return
    ''                  a dictionary where the key holds the property name and the value its value. Example of a reflect function within a User class which has firstname and lastname properties
    ''                  <code>
    ''                  <%
    ''                  function reflect()
    ''                  .   set reflect = server.createObject("scripting.dictionary")
    ''                  .   reflect.add "firstname", firstname
    ''                  .   reflect.add "lastname", lastname
    ''                  end function
    ''                  % >
    ''                  </code>
    ''                  Example of how to generate a JSON representation of the asp request object and access the <em>HTTP_HOST</em> server variable in JavaScript:
    ''                  <code>
    ''                  <script>alert(<%= (new JSON)(empty, request, false) % >.servervariables.HTTP_HOST);</script>
    ''                  </code>
    '' @PARAM:          nested [bool]: indicates if the name value pair is already nested within another? if yes then the <em>{}</em> are left out.
    '' @RETURN:         [string] returns a JSON representation of the given name value pair
    ''                  (if toResponse is on then the return is written directly to the response and nothing is returned)
    '******************************************************************************************************************
    public default function toJSON(name, val, nested)
        if not nested and not isEmpty(name) then write("{")
        if not isEmpty(name) then write("""" & escape(name) & """: ")
        generateValue(val)
        if not nested and not isEmpty(name) then write("}")
        toJSON = output

        if innerCall = 0 then newGeneration()
    end function

    '******************************************************************************************************************
    '* generate
    '******************************************************************************************************************
    private function generateValue(val)
        if isNull(val) then
            write("null")
        elseif isArray(val) then
            generateArray(val)
        elseif isObject(val) then
            dim tName : tName = typename(val)
            if val is nothing then
                write("null")
            elseif tName = "Dictionary" or tName = "IRequestDictionary" then
                generateDictionary(val)
            elseif tName = "Recordset" then
                generateRecordset(val)
            elseif tName = "IRequest" then
                set req = server.createObject("scripting.dictionary")
                req.add "clientcertificate", val.ClientCertificate
                req.add "cookies", val.cookies
                req.add "form", val.form
                req.add "querystring", val.queryString
                req.add "servervariables", val.serverVariables
                req.add "totalbytes", val.totalBytes
                generateDictionary(req)
            elseif tName = "IStringList" then
                if val.count = 1 then
                    toJSON empty, val(1), true
                else
                    generateArray(val)
                end if
            else
                generateObject(val)
            end if
        else
            'bool
            dim varTyp
            varTyp = varType(val)
            if varTyp = 11 then
                if val then write("true") else write("false")
            'int, long, byte
            elseif varTyp = 2 or varTyp = 3 or varTyp = 17 or varTyp = 19 then
                write(cLng(val))
            'single, double, currency
            elseif varTyp = 4 or varTyp = 5 or varTyp = 6 or varTyp = 14 then
                write(replace(cDbl(val), ",", "."))
            else
                write("""" & escape(val & "") & """")
            end if
        end if
        generateValue = output
    end function

    '******************************************************************************************************************
    '* generateArray
    '******************************************************************************************************************
    private sub generateArray(val)
        dim item, i
        write("[")
        i = 0
        'the for each allows us to support also multi dimensional arrays
        for each item in val
            if i > 0 then write(",")
            generateValue(item)
            i = i + 1
        next
        write("]")
    end sub

    '******************************************************************************************************************
    '* generateDictionary
    '******************************************************************************************************************
    private sub generateDictionary(val)
        innerCall = innerCall + 1
        if val.count = 0 then
            toJSON empty, null, true
            exit sub
        end if
        dim key, i
        write("{")
        i = 0
        for each key in val
            if i > 0 then write(",")
            toJSON key, val(key), true
            i = i + 1
        next
        write("}")
        innerCall = innerCall - 1
    end sub

    '******************************************************************************************************************
    '* generateRecordset
    '******************************************************************************************************************
    private sub generateRecordset(val)
        dim i, curRow
        write("[")
        curRow = 0
        'recordset.pagesize = -1 means it is not paged.
        while not val.eof and ((recordsetPaging and curRow < val.pageSize) or val.recordCount = -1 or not recordsetPaging)
            innerCall = innerCall + 1
            write("{")
            for i = 0 to val.fields.count - 1
                if i > 0 then write(",")
                toJSON lCase(val.fields(i).name), val.fields(i).value, true
            next
            write("}")
            val.movenext()
            curRow = curRow + 1
            if not val.eof and ((recordsetPaging and curRow < val.pageSize) or val.recordCount = -1 or not recordsetPaging) then write(",")
            innerCall = innerCall - 1
        wend
        write("]")
    end sub

    '******************************************************************************************************************
    '* generateObject
    '******************************************************************************************************************
    private sub generateObject(val)
        dim props
        on error resume next
        set props = val.reflect()
        if err = 0 then
            on error goto 0
            innerCall = innerCall + 1
            toJSON empty, props, true
            innerCall = innerCall - 1
        else
            on error goto 0
            write("""" & escape(typename(val)) & """")
        end if
    end sub

    '******************************************************************************************************************
    '* newGeneration
    '******************************************************************************************************************
    private sub newGeneration()
        output = empty
        innerCall = 0
    end sub

    '******************************************************************************************
    '* JsonEscapeSquence
    '******************************************************************************************
    private function escapequence(digit)
        escapequence = "\u00" + right(padLeft(hex(ascw(digit)), 2, 0), 2)
    end function

    '******************************************************************************************
    '* padLeft
    '******************************************************************************************
    private function padLeft(value, totalLength, paddingChar)
        padLeft = right(clone(paddingChar, totalLength) & value, totalLength)
    end function

    '******************************************************************************************
    '* clone
    '******************************************************************************************
    private function clone(byVal str, n)
        dim i
        for i = 1 to n : clone = clone & str : next
    end function

    '******************************************************************************************
    '* write
    '******************************************************************************************
    private sub write(val)
        if toResponse then
            response.write(val)
        else
            output = output & val
        end if
    end sub

end class
%>
```

---

### json.objectClass.asp

```asp
<%
' JSON object class 3.8.1 May, 29th - 2016
' https://github.com/rcdmk/aspJSON
'
' License MIT - see LICENCE.txt for details

const JSON_ROOT_KEY = "[[JSONroot]]"
const JSON_DEFAULT_PROPERTY_NAME = "data"
const JSON_SPECIAL_VALUES_REGEX = "^(?:(?:t(?:r(?:ue?)?)?)|(?:f(?:a(?:l(?:se?)?)?)?)|(?:n(?:u(?:ll?)?))|(?:u(?:n(?:d(?:e(?:f(?:i(?:n(?:ed?)?)?)?)?)?)?)?))$"

const JSON_ERROR_PARSE = 1
const JSON_ERROR_PROPERTY_ALREADY_EXISTS = 2
const JSON_ERROR_PROPERTY_DOES_NOT_EXISTS = 3 ' DEPRECATED
const JSON_ERROR_NOT_AN_ARRAY = 4
const JSON_ERROR_NOT_A_STRING = 5
const JSON_ERROR_INDEX_OUT_OF_BOUNDS = 9 ' Numbered to have the same error number as the default "Subscript out of range" exeption

class JSONobject
    dim i_debug, i_depth, i_parent
    dim i_properties, i_version, i_defaultPropertyName
    private vbback

    ' Set to true to show the internals of the parsing mecanism
    public property get debug
        debug = i_debug
    end property

    public property let debug(value)
        i_debug = value
    end property


    ' Gets/sets the default property name generated when loading recordsets and arrays (default "data")
    public property get defaultPropertyName
        defaultPropertyName = i_defaultPropertyName
    end property

    public property let defaultPropertyName(value)
        i_defaultPropertyName = value
    end property


    ' The depth of the object in the chain, starting with 1
    public property get depth
        depth = i_depth
    end property


    ' The property pairs ("name": "value" - pairs)
    public property get pairs
        pairs = i_properties
    end property


    ' The parent object
    public property get parent
        set parent = i_parent
    end property

    public property set parent(value)
        set i_parent = value
        i_depth = i_parent.depth + 1
    end property



    ' Constructor and destructor
    private sub class_initialize()
        i_version = "3.8.1"
        i_depth = 0
        i_debug = false
        i_defaultPropertyName = JSON_DEFAULT_PROPERTY_NAME

        set i_parent = nothing
        redim i_properties(-1)

        vbback = Chr(8)
    end sub

    private sub class_terminate()
        dim i
        for i = 0 to ubound(i_properties)
            set i_properties(i) = nothing
        next

        redim i_properties(-1)
    end sub


    ' Parse a JSON string and populate the object
    public function parse(byval strJson)
        dim regex, i, size, char, prevchar, quoted
        dim mode, item, key, value, openArray, openObject
        dim actualLCID, tmpArray, tmpObj, addedToArray
        dim root, currentObject, currentArray

        log("Load string: """ & strJson & """")

        ' Store the actual LCID and use the en-US to conform with the JSON standard
        actualLCID = Response.LCID
        Response.LCID = 1033

        strJson = trim(strJson)

        size = len(strJson)

        ' At least 2 chars to continue
        if size < 2 then err.raise JSON_ERROR_PARSE, TypeName(me), "Invalid JSON string to parse"

        ' Init the regex to be used in the loop
        set regex = new regexp
        regex.global = true
        regex.ignoreCase = true
        regex.pattern = "\w"

        ' setup initial values
        i = 0
        set root = me
        key = JSON_ROOT_KEY
        mode = "init"
        quoted = false
        set currentObject = root

        ' main state machine
        do while i < size
            i = i + 1
            char = mid(strJson, i, 1)

            ' root, object or array start
            if mode = "init" then
                log("Enter init")

                ' if we are in root, clear previous object properties
                if key = JSON_ROOT_KEY and TypeName(currentArray) <> "JSONarray" then redim i_properties(-1)

                ' Init object
                if char = "{" then
                    log("Create object<ul>")

                    if key <> JSON_ROOT_KEY or TypeName(root) = "JSONarray" then
                        ' creates a new object
                        set item = new JSONobject
                        set item.parent = currentObject

                        addedToArray = false

                        ' Object is inside an array
                        if TypeName(currentArray) = "JSONarray" then
                            if currentArray.depth > currentObject.depth then
                                ' Add it to the array
                                set item.parent = currentArray
                                currentArray.Push item

                                addedToArray = true

                                log("Added to the array")
                            end if
                        end if

                        if not addedToArray then
                            currentObject.add key, item
                            log("Added to parent object: """ & key & """")
                        end if

                        set currentObject = item
                    end if

                    openObject = openObject + 1
                    mode = "openKey"

                ' Init Array
                elseif char = "[" then
                    log("Create array<ul>")

                    set item = new JSONarray

                    addedToArray = false

                    ' Array is inside an array
                    if isobject(currentArray) and openArray > 0 then
                        if currentArray.depth > currentObject.depth then
                            ' Add it to the array
                            set item.parent = currentArray
                            currentArray.Push item

                            addedToArray = true

                            log("Added to parent array")
                        end if
                    end if

                    if not addedToArray then
                        set item.parent = currentObject
                        currentObject.add key, item
                        log("Added to parent object")
                    end if

                    if key = JSON_ROOT_KEY and item.depth = 1 then
                        set root = item
                        log("Set as root")
                    end if

                    set currentArray = item
                    openArray = openArray + 1
                    mode = "openValue"
                end if

            ' Init a key
            elseif mode = "openKey" then
                key = ""
                if char = """" then
                    log("Open key")
                    mode = "closeKey"
                elseif char = "}" then ' empty objects
                    log("Empty object")
                    mode = "next"
                    i = i - 1 ' we backup one char to make the next iteration get the closing bracket
                end if

            ' Fill in the key until finding a double quote "
            elseif mode = "closeKey" then
                ' If it finds a non scaped quotation, change to value mode
                if char = """" and prevchar <> "\" then
                    log("Close key: """ & key & """")
                    mode = "preValue"
                else
                    key = key & char
                end if

            ' Wait until a colon char (:) to begin the value
            elseif mode = "preValue" then
                if char = ":" then
                    mode = "openValue"
                    log("Open value for """ & key & """")
                end if

            ' Begining of value
            elseif mode = "openValue" then
                value = ""

                ' If the next char is a closing square barcket (]), its closing an empty array
                if char = "]" then
                    log("Closing empty array")
                    quoted = false
                    mode = "next"
                    i = i - 1 ' we backup one char to make the next iteration get the closing bracket

                ' If it begins with a double quote, its a string value
                elseif char = """" then
                    log("Open string value")
                    quoted = true
                    mode = "closeValue"

                ' If it begins with open square bracket ([), its an array
                elseif char = "[" then
                    log("Open array value")
                    quoted = false
                    mode = "init"
                    i = i - 1 ' we backup one char to init with '['

                ' If it begins with open a bracket ({), its an object
                elseif char = "{" then
                    log("Open object value")
                    quoted = false
                    mode = "init"
                    i = i - 1 ' we backup one char to init with '{'

                else
                    ' If its a number, start a numeric value
                    if regex.pattern <> "\d" then regex.pattern = "\d"
                    if regex.test(char) then
                        log("Open numeric value")
                        quoted = false
                        value = char
                        mode = "closeValue"
                        if prevchar = "-" then
                            value = prevchar & char
                        end if

                    ' special values: null, true, false and undefined
                    elseif char = "n" or char = "t" or char = "f" or char = "u" then
                        log("Open special value")
                        quoted = false
                        value = char
                        mode = "closeValue"
                    end if
                end if

            ' Fill in the value until finish
            elseif mode = "closeValue" then
                if quoted then
                    if char = """" and prevchar <> "\" then
                        log("Close string value: """ & value & """")
                        mode = "addValue"

                    ' special and escaped chars
                    elseif prevchar = "\" then
                        select case char
                            case "n"
                                value = value & vblf
                            case "r"
                                value = value & vbcr
                            case "t"
                                value = value & vbtab
                            case "b"
                                value = value & vbback

                            ' escaped chars fix by @IT-Portal
                            case "\"
                                'for \\t we must have \t (not \tab)
                                'here we're resetting prevchar for next iteration
                                value = value & char
                                char = ""

                            ' escaped unicode syntax by @IT-Portal
                            case "u"
                                '\uxxxx support
                                if IsNumeric("&H" & mid(strJson, i + 1, 4)) then
                                    value = value & ChrW("&H" & mid(strJson, i + 1, 4))
                                    i = i + 4
                                else
                                    value = value & char
                                end if

                            case else
                                value = value & char
                        end select
                    elseif char <> "\" then
                        value = value & char
                    end if
                else
                    ' possible boolean and null values
                    if regex.pattern <> JSON_SPECIAL_VALUES_REGEX then regex.pattern = JSON_SPECIAL_VALUES_REGEX
                    if regex.test(char) or regex.test(value) then
                        value = value & char
                        if value = "true" or value = "false" or value = "null" or value = "undefined" then mode = "addValue"
                    else
                        char = lcase(char)

                        ' If is a numeric char
                        if regex.pattern <> "\d" then regex.pattern = "\d"
                        if regex.test(char) then
                            value = value & char

                        ' If it's not a numeric char, but the prev char was a number
                        ' used to catch separators and special numeric chars
                        elseif regex.test(prevchar) or prevchar = "e" then
                            if char = "." or char = "e" or (prevchar = "e" and (char = "-" or char = "+")) then
                                value = value & char
                            else
                                log("Close numeric value: " & value)
                                mode = "addValue"
                                i = i - 1
                            end if
                        else
                            log("Close numeric value: " & value)
                            mode = "addValue"
                            i = i - 1
                        end if
                    end if
                end if

            ' Add the value to the object or array
            elseif mode = "addValue" then
                if key <> "" then
                    dim useArray
                    useArray = false

                    if not quoted then
                        if isNumeric(value) then
                            log("Value converted to number")
                            value = cdbl(value)
                        else
                            log("Value converted to " & value)
                            value = eval(value)
                        end if
                    end if

                    quoted = false

                    ' If it's inside an array
                    if openArray > 0 and isObject(currentArray) then
                        useArray = true

                        ' If it's a property of an object that is inside the array
                        ' we add it to the object instead
                        if isObject(currentObject) then
                            if currentObject.depth >= currentArray.depth + 1 then useArray = false
                        end if

                        ' else, we add it to the array
                        if useArray then
                            tmpArray = currentArray.items
                            ArrayPush tmpArray, value

                            currentArray.items = tmpArray

                            log("Value added to array: """ & key & """: " & value)
                        end if
                    end if

                    if not useArray then
                        currentObject.add key, value
                        log("Value added: """ & key & """")
                    end if
                end if

                mode = "next"
                i = i - 1

            ' Change the current mode according to the current state
            elseif mode = "next" then
                if char = "," then
                    ' If it's an array
                    if openArray > 0 and isObject(currentArray) then
                        ' and the current object is a parent or sibling object
                        if currentArray.depth >= currentObject.depth then
                            ' start an array value
                            log("New value")
                            mode = "openValue"
                        else
                            ' start an object key
                            log("New key")
                            mode = "openKey"
                        end if
                    else
                        ' start an object key
                        log("New key")
                        mode = "openKey"
                    end if

                elseif char = "]" then
                    log("Close array</ul>")

                    ' If it's and open array, we close it and set the current array as its parent
                    if isobject(currentArray.parent) then
                        if TypeName(currentArray.parent) = "JSONarray" then
                            set currentArray = currentArray.parent

                        ' if the parent is an object
                        elseif TypeName(currentArray.parent) = "JSONobject" then
                            set tmpObj = currentArray.parent

                            ' we search for the next parent array to set the current array
                            while isObject(tmpObj) and TypeName(tmpObj) = "JSONobject"
                                if isObject(tmpObj.parent) then
                                    set tmpObj = tmpObj.parent
                                else
                                    tmpObj = tmpObj.parent
                                end if
                            wend

                            set currentArray = tmpObj
                        end if
                    else
                        currentArray = currentArray.parent
                    end if

                    openArray = openArray - 1

                    mode = "next"

                elseif char = "}" then
                    log("Close object</ul>")

                    ' If it's an open object, we close it and set the current object as it's parent
                    if isobject(currentObject.parent) then
                        if TypeName(currentObject.parent) = "JSONobject" then
                            set currentObject = currentObject.parent

                        ' If the parent is and array
                        elseif TypeName(currentObject.parent) = "JSONarray" then
                            set tmpObj = currentObject.parent

                            ' we search for the next parent object to set the current object
                            while isObject(tmpObj) and TypeName(tmpObj) = "JSONarray"
                                set tmpObj = tmpObj.parent
                            wend

                            set currentObject = tmpObj
                        end if
                    else
                        currentObject = currentObject.parent
                    end if

                    openObject = openObject - 1

                    mode = "next"
                end if
            end if

            prevchar = char
        loop

        set regex = nothing

        Response.LCID = actualLCID

        set parse = root
    end function

    ' Add a new property (key-value pair)
    public sub add(byval prop, byval obj)
        dim p
        getProperty prop, p

        if GetTypeName(p) = "JSONpair" then
            err.raise JSON_ERROR_PROPERTY_ALREADY_EXISTS, TypeName(me), "A property already exists with the name: " & prop & "."
        else
            dim item
            set item = new JSONpair
            item.name = prop
            set item.parent = me

            dim itemType
            itemType = GetTypeName(obj)

            if isArray(obj) then
                dim item2
                set item2 = new JSONarray
                item2.items = obj
                set item2.parent = me

                set item.value = item2

            elseif itemType = "Field" then
                item.value = obj.value
            elseif isObject(obj) and itemType <> "IStringList" then
                set item.value = obj
            else
                item.value = obj
            end if

            ArrayPush i_properties, item
        end if
    end sub

    ' Remove a property from the object (key-value pair)
    public sub remove(byval prop)
        dim p, i
        i = getProperty(prop, p)

        ' property exists
        if i > -1 then ArraySlice i_properties, i
    end sub

    ' Return the value of a property by its key
    public default function value(byval prop)
        dim p
        getProperty prop, p

        if GetTypeName(p) = "JSONpair" then
            if isObject(p.value) then
                set value = p.value
            else
                value = p.value
            end if
        else
            value = null
        end if
    end function

    ' Change the value of a property
    ' Creates the property if it didn't exists
    public sub change(byval prop, byval obj)
        dim p
        getProperty prop, p

        if GetTypeName(p) = "JSONpair" then
            if isArray(obj) then
                set item = new JSONarray
                item.items = obj
                set item.parent = me

                p.value = item

            elseif isObject(obj) then
                set p.value = obj
            else
                p.value = obj
            end if
        else
            add prop, obj
        end if
    end sub

    ' Returns the index of a property if it exists, else -1
    ' @param prop as string - the property name
    ' @param out outProp as variant - will be filled with the property value, nothing if not found
    private function getProperty(byval prop, byref outProp)
        dim i, p, found
        set outProp = nothing
        found = false

        i = 0

        do while i <= ubound(i_properties)
            set p = i_properties(i)

            if p.name = prop then
                set outProp = p
                found = true

                exit do
            end if

            i = i + 1
        loop

        if not found then i = -1

        getProperty = i
    end function


    ' Serialize the current object to a JSON formatted string
    public function Serialize()
        dim actualLCID, out
        actualLCID = Response.LCID
        Response.LCID = 1033

        out = serializeObject(me)

        Response.LCID = actualLCID

        Serialize = out
    end function

    ' Writes the JSON serialized object to the response
    public sub write()
        response.write Serialize
    end sub


    ' Helpers
    ' Serializes a JSON object to JSON formatted string
    public function serializeObject(obj)
        dim out, prop, value, i, pairs, valueType
        out = "{"

        pairs = obj.pairs

        for i = 0 to ubound(pairs)
            set prop = pairs(i)

            if out <> "{" then out = out & ","

            if isobject(prop.value) then
                set value = prop.value
            else
                value = prop.value
            end if

            if prop.name = JSON_ROOT_KEY then
                out = out & """" & obj.defaultPropertyName & """:"
            else
                out = out & """" & prop.name & """:"
            end if

            if isArray(value) or GetTypeName(value) = "JSONarray" then
                out = out & serializeArray(value)

            elseif isObject(value) and GetTypeName(value) = "JSONscript" then
                out = out & value.Serialize()

            elseif isObject(value) then
                out = out & serializeObject(value)

            else
                out = out & serializeValue(value)
            end if
        next

        out = out & "}"

        serializeObject = out
    end function

    ' Serializes a value to a valid JSON formatted string representing the value
    ' (quoted for strings, the type name for objects, null for nothing and null values)
    public function serializeValue(byval value)
        dim out

        select case lcase(GetTypeName(value))
            case "null"
                out = "null"

            case "nothing"
                out = "undefined"

            case "boolean"
                if value then
                    out = "true"
                else
                    out = "false"
                end if

            case "byte", "integer", "long", "single", "double", "currency", "decimal"
                out = value

            case "date"
                out = """" & year(value) & "-" & padZero(month(value), 2) & "-" & padZero(day(value), 2) & "T" & padZero(hour(value), 2) & ":" & padZero(minute(value), 2) & ":" & padZero(second(value), 2) & """"

            case "string", "char", "empty"
                out = """" & EscapeCharacters(value) & """"

            case else
                out = """" & GetTypeName(value) & """"
        end select

        serializeValue = out
    end function

    ' Pads a numeric string with zeros at left
    private function padZero(byval number, byval length)
        dim result
        result = number

        while len(result) < length
            result = "0" & result
        wend

        padZero = result
    end function

    ' Serializes an array item to JSON formatted string
    private function serializeArrayItem(byref elm)
        dim out, val

        if isobject(elm) then
            if GetTypeName(elm) = "JSONobject" then
                set val = elm

            elseif GetTypeName(elm) = "JSONarray" then
                val = elm.items

            elseif isObject(elm.value) then
                set val = elm.value

            else
                val = elm.value
            end if
        else
            val = elm
        end if

        if isArray(val) or GetTypeName(val) = "JSONarray" then
            out = out & serializeArray(val)

        elseif isObject(val) then
            out = out & serializeObject(val)

        else
            out = out & serializeValue(val)
        end if

        serializeArrayItem = out
    end function

    ' Serializes an array or JSONarray object to JSON formatted string
    public function serializeArray(byref arr)
        dim i, j, k, dimensions, out, innerArray, elm, val

        out = "["

        if isobject(arr) then
            log("Serializing jsonArray object")
            innerArray = arr.items
        else
            log("Serializing VB array")
            innerArray = arr
        end if

        dimensions = NumDimensions(innerArray)

        if dimensions > 1 then
            log("Multidimensional array")
            for j = 0 to ubound(innerArray, 1)
                out = out & "["

                for k = 0 to ubound(innerArray, 2)
                    if k > 0 then out = out & ","

                    if isObject(innerArray(j, k)) then
                        set elm = innerArray(j, k)
                    else
                        elm = innerArray(j, k)
                    end if

                    out = out & serializeArrayItem(elm)
                next

                out = out & "]"
            next
        else
            for j = 0 to ubound(innerArray)
                if j > 0 then out = out & ","

                if isobject(innerArray(j)) then
                    set elm = innerArray(j)
                else
                    elm = innerArray(j)
                end if

                out = out & serializeArrayItem(elm)
            next
        end if

        out = out & "]"

        serializeArray = out
    end function


    ' Returns the number of dimensions an array has
    public Function NumDimensions(byref arr)
        Dim dimensions
        dimensions = 0

        On Error Resume Next

        Do While Err.number = 0
            dimensions = dimensions + 1
            UBound arr, dimensions
        Loop
        On Error Goto 0

        NumDimensions = dimensions - 1
    End Function

    ' Pushes (adds) a value to an array, expanding it
    public function ArrayPush(byref arr, byref value)
        redim preserve arr(ubound(arr) + 1)

        if isobject(value) then
            set arr(ubound(arr)) = value
        else
            arr(ubound(arr)) = value
        end if

        ArrayPush = arr
    end function

    ' Removes a value from an array
    private function ArraySlice(byref arr, byref index)
        dim i, upperBound
        i = index
        upperBound = ubound(arr)

        do while i < upperBound
            if isObject(arr(i)) then
                set arr(i) = arr(i + 1)
            else
                arr(i) = arr(i + 1)
            end if

            i = i + 1
        loop

        redim preserve arr(upperBound - 1)

        ArraySlice = arr
    end function

    ' Load properties from an ADO RecordSet object into an array
    ' @param rs as ADODB.RecordSet
    public sub LoadRecordSet(byref rs)
        dim arr, obj, field

        set arr = new JSONArray

        while not rs.eof
            set obj = new JSONobject

            for each field in rs.fields
                obj.Add field.name, field.value
            next

            arr.Push obj

            rs.movenext
        wend

        set obj = nothing

        add JSON_ROOT_KEY, arr
    end sub

    ' Load properties from the first record of an ADO RecordSet object
    ' @param rs as ADODB.RecordSet
    public sub LoadFirstRecord(byref rs)
        dim field

        for each field in rs.fields
            add field.name, field.value
        next
    end sub

    ' Returns the value's type name (usefull for types not supported by VBS)
    public function GetTypeName(byval value)
        dim valueType

        on error resume next
            valueType = TypeName(value)

            if err.number <> 0 then
                if varType(value) = 14 then valueType = "Decimal"
            end if
        on error goto 0

        GetTypeName = valueType
    end function

    ' Escapes special characters in the text
    ' @param text as String
    public function EscapeCharacters(byval text)
        dim result

        result = text

        if not isNull(text) then
            result = cstr(result)

            result = replace(result, "\", "\\")
            result = replace(result, """", "\""")
            result = replace(result, vbcr, "\r")
            result = replace(result, vblf, "\n")
            result = replace(result, vbtab, "\t")
            result = replace(result, vbback, "\b")
        end if

        EscapeCharacters = result
    end function

    ' Used to write the log messages to the response on debug mode
    private sub log(byval msg)
        if i_debug then response.write "<li>" & msg & "</li>" & vbcrlf
    end sub
end class


' JSON array class
' Represents an array of JSON objects and values
class JSONarray
    dim i_items, i_depth, i_parent, i_version, i_defaultPropertyName

    ' The class version
    public property get version
        version = i_version
    end property

    ' The actual array items
    public property get items
        items = i_items
    end property

    public property let items(value)
        if isArray(value) then
            i_items = value
        else
            err.raise JSON_ERROR_NOT_AN_ARRAY, TypeName(me), "The value assigned is not an array."
        end if
    end property

    ' The length of the array
    public property get length
        length = ubound(i_items) + 1
    end property

    ' The depth of the array in the chain (starting with 1)
    public property get depth
        depth = i_depth
    end property

    ' The parent object or array
    public property get parent
        set parent = i_parent
    end property

    public property set parent(value)
        set i_parent = value
        i_depth = i_parent.depth + 1
        i_defaultPropertyName = i_parent.defaultPropertyName
    end property

    ' Gets/sets the default property name generated when loading recordsets and arrays (default "data")
    public property get defaultPropertyName
        defaultPropertyName = i_defaultPropertyName
    end property

    public property let defaultPropertyName(value)
        i_defaultPropertyName = value
    end property



    ' Constructor and destructor
    private sub class_initialize
        i_version = "2.3.5"
        i_defaultPropertyName = JSON_DEFAULT_PROPERTY_NAME
        redim i_items(-1)
        i_depth = 0
    end sub

    private sub class_terminate
        dim i, j, js, dimensions

        dimensions = 0

        On Error Resume Next

        Do While Err.number = 0
            dimensions = dimensions + 1
            UBound i_items, dimensions
        Loop

        On Error Goto 0

        dimensions = dimensions - 1

        for i = 1 to dimensions
            for j = 0 to ubound(i_items, i)
                if dimensions = 1 then
                    set i_items(j) = nothing
                else
                    set i_items(i - 1, j) = nothing
                end if
            next
        next
    end sub

    ' Adds a value to the array
    public sub Push(byref value)
        dim js, instantiated

        if typeName(i_parent) = "JSONobject" then
            set js = i_parent
            i_defaultPropertyName = i_parent.defaultPropertyName
        else
            set js = new JSONobject
            js.defaultPropertyName = i_defaultPropertyName
            instantiated = true
        end if

        js.ArrayPush i_items, value

        if instantiated then set js = nothing
    end sub

    ' Load properties from a ADO RecordSet object
    public sub LoadRecordSet(byref rs)
        dim obj, field

        while not rs.eof
            set obj = new JSONobject

            for each field in rs.fields
                obj.Add field.name, field.value
            next

            Push obj

            rs.movenext
        wend

        set obj = nothing
    end sub

    ' Returns the item at the specified index
    ' @param index as int - the desired item index
    public default function ItemAt(byval index)
        dim len
        len = me.length

        if len > 0 and index < len then
            if isObject(i_items(index)) then
                set ItemAt = i_items(index)
            else
                ItemAt = i_items(index)
            end if
        else
            err.raise JSON_ERROR_INDEX_OUT_OF_BOUNDS, TypeName(me), "Index out of bounds."
        end if
    end function

    ' Serializes this JSONarray object in JSON formatted string value
    ' (uses the JSONobject.SerializeArray method)
    public function Serialize()
        dim js, out, instantiated, actualLCID

        actualLCID = Response.LCID
        Response.LCID = 1033

        if not isEmpty(i_parent) then
            if TypeName(i_parent) = "JSONobject" then
                set js = i_parent
                i_defaultPropertyName = i_parent.defaultPropertyName
            end if
        end if

        if isEmpty(js) then
            set js = new JSONobject
            js.defaultPropertyName = i_defaultPropertyName
            instantiated = true
        end if

        out = js.SerializeArray(me)

        if instantiated then set js = nothing

        Response.LCID = actualLCID

        Serialize = out
    end function

    ' Writes the serialized array to the response
    public function Write()
        Response.Write Serialize()
    end function
end class


class JSONscript
    dim i_version
    dim s_value, s_nullString

    ' The value
    public property get value
        value = s_value
    end property

    public property let value(newValue)
        if (TypeName(newValue) <> "String") then
            err.raise JSON_ERROR_NOT_A_STRING, TypeName(me), "The value assigned is not a string."
        end if

        if (len(newValue) = 0) then newValue = s_nullString
        s_value = newValue
    end property

    ' Constructor and destructor
    private sub class_initialize()
        i_version = "1.0.0"

        s_nullString = "null"
        s_value = s_nullString
    end sub

    ' Serializes this object by outputting the raw value
    public function Serialize()
        Serialize = s_value
    end function

    ' Writes the serialized object to the response
    public function Write()
        Response.Write Serialize()
    end function
end class


' JSON pair class
' represents a name/value pair of a JSON object
class JSONpair
    dim i_name, i_value
    dim i_parent

    ' The name or key of the pair
    public property get name
        name = i_name
    end property

    public property let name(val)
        i_name = val
    end property

    ' The value of the pair
    public property get value
        if isObject(i_value) then
            set value = i_value
        else
            value = i_value
        end if
    end property

    public property let value(val)
        i_value = val
    end property

    public property set value(val)
        set i_value = val
    end property

    ' The parent object
    public property get parent
        set parent = i_parent
    end property

    public property set parent(val)
        set i_parent = val
    end property


    ' Constructor and destructor
    private sub class_initialize
    end sub

    private sub class_terminate
        if isObject(value) then set value = nothing
    end sub
end class
%>
```

---

### aspJSON.asp

```asp
<%
'Februari 2014 - Version 1.17 by Gerrit van Kuipers
Class aspJSON
    Public data
    Private p_JSONstring
    private aj_in_string, aj_in_escape, aj_i_tmp, aj_char_tmp, aj_s_tmp, aj_line_tmp, aj_line, aj_lines, aj_currentlevel, aj_currentkey, aj_currentvalue, aj_newlabel, aj_XmlHttp, aj_RegExp, aj_colonfound

    Private Sub Class_Initialize()
        Set data = Collection()

        Set aj_RegExp = new regexp
        aj_RegExp.Pattern = "\s{0,}(\S{1}[\s,\S]*\S{1})\s{0,}"
        aj_RegExp.Global = False
        aj_RegExp.IgnoreCase = True
        aj_RegExp.Multiline = True
    End Sub

    Private Sub Class_Terminate()
        Set data = Nothing
        Set aj_RegExp = Nothing
    End Sub

    Public Sub loadJSON(inputsource)
        inputsource = aj_MultilineTrim(inputsource)
        If Len(inputsource) = 0 Then Err.Raise 1, "loadJSON Error", "No data to load."

        select case Left(inputsource, 1)
            case "{", "["
            case else
                Set aj_XmlHttp = Server.CreateObject("Msxml2.ServerXMLHTTP")
                aj_XmlHttp.open "GET", inputsource, False
                aj_XmlHttp.setRequestHeader "Content-Type", "text/json"
                aj_XmlHttp.setRequestHeader "CharSet", "UTF-8"
                aj_XmlHttp.Send
                inputsource = aj_XmlHttp.responseText
                set aj_XmlHttp = Nothing
        end select

        p_JSONstring = CleanUpJSONstring(inputsource)
        aj_lines = Split(p_JSONstring, Chr(13) & Chr(10))

        Dim level(99)
        aj_currentlevel = 1
        Set level(aj_currentlevel) = data
        For Each aj_line In aj_lines
            aj_currentkey = ""
            aj_currentvalue = ""
            If Instr(aj_line, ":") > 0 Then
                aj_in_string = False
                aj_in_escape = False
                aj_colonfound = False
                For aj_i_tmp = 1 To Len(aj_line)
                    If aj_in_escape Then
                        aj_in_escape = False
                    Else
                        Select Case Mid(aj_line, aj_i_tmp, 1)
                            Case """"
                                aj_in_string = Not aj_in_string
                            Case ":"
                                If Not aj_in_escape And Not aj_in_string Then
                                    aj_currentkey = Left(aj_line, aj_i_tmp - 1)
                                    aj_currentvalue = Mid(aj_line, aj_i_tmp + 1)
                                    aj_colonfound = True
                                    Exit For
                                End If
                            Case "\"
                                aj_in_escape = True
                        End Select
                    End If
                Next
                if aj_colonfound then
                    aj_currentkey = aj_Strip(aj_JSONDecode(aj_currentkey), """")
                    If Not level(aj_currentlevel).exists(aj_currentkey) Then level(aj_currentlevel).Add aj_currentkey, ""
                end if
            End If
            If right(aj_line,1) = "{" Or right(aj_line,1) = "[" Then
                If Len(aj_currentkey) = 0 Then aj_currentkey = level(aj_currentlevel).Count
                Set level(aj_currentlevel).Item(aj_currentkey) = Collection()
                Set level(aj_currentlevel + 1) = level(aj_currentlevel).Item(aj_currentkey)
                aj_currentlevel = aj_currentlevel + 1
                aj_currentkey = ""
            ElseIf right(aj_line,1) = "}" Or right(aj_line,1) = "]" or right(aj_line,2) = "}," Or right(aj_line,2) = "]," Then
                aj_currentlevel = aj_currentlevel - 1
            ElseIf Len(Trim(aj_line)) > 0 Then
                if Len(aj_currentvalue) = 0 Then aj_currentvalue = aj_line
                aj_currentvalue = getJSONValue(aj_currentvalue)

                If Len(aj_currentkey) = 0 Then aj_currentkey = level(aj_currentlevel).Count
                level(aj_currentlevel).Item(aj_currentkey) = aj_currentvalue
            End If
        Next
    End Sub

    Public Function Collection()
        set Collection = Server.CreateObject("Scripting.Dictionary")
    End Function

    Public Function AddToCollection(dictobj)
        if TypeName(dictobj) <> "Dictionary" then Err.Raise 1, "AddToCollection Error", "Not a collection."
        aj_newlabel = dictobj.Count
        dictobj.Add aj_newlabel, Collection()
        set AddToCollection = dictobj.item(aj_newlabel)
    end function

    Private Function CleanUpJSONstring(aj_originalstring)
        aj_originalstring = Replace(aj_originalstring, Chr(13) & Chr(10), "")
        aj_originalstring = Mid(aj_originalstring, 2, Len(aj_originalstring) - 2)
        aj_in_string = False : aj_in_escape = False : aj_s_tmp = ""
        For aj_i_tmp = 1 To Len(aj_originalstring)
            aj_char_tmp = Mid(aj_originalstring, aj_i_tmp, 1)
            If aj_in_escape Then
                aj_in_escape = False
                aj_s_tmp = aj_s_tmp & aj_char_tmp
            Else
                Select Case aj_char_tmp
                    Case "\" : aj_s_tmp = aj_s_tmp & aj_char_tmp : aj_in_escape = True
                    Case """" : aj_s_tmp = aj_s_tmp & aj_char_tmp : aj_in_string = Not aj_in_string
                    Case "{", "["
                        aj_s_tmp = aj_s_tmp & aj_char_tmp & aj_InlineIf(aj_in_string, "", Chr(13) & Chr(10))
                    Case "}", "]"
                        aj_s_tmp = aj_s_tmp & aj_InlineIf(aj_in_string, "", Chr(13) & Chr(10)) & aj_char_tmp
                    Case "," : aj_s_tmp = aj_s_tmp & aj_char_tmp & aj_InlineIf(aj_in_string, "", Chr(13) & Chr(10))
                    Case Else : aj_s_tmp = aj_s_tmp & aj_char_tmp
                End Select
            End If
        Next

        CleanUpJSONstring = ""
        aj_s_tmp = split(aj_s_tmp, Chr(13) & Chr(10))
        For Each aj_line_tmp In aj_s_tmp
            aj_line_tmp = replace(replace(aj_line_tmp, chr(10), ""), chr(13), "")
            CleanUpJSONstring = CleanUpJSONstring & aj_Trim(aj_line_tmp) & Chr(13) & Chr(10)
        Next
    End Function

    Private Function getJSONValue(ByVal val)
        val = Trim(val)
        If Left(val,1) = ":"  Then val = Mid(val, 2)
        If Right(val,1) = "," Then val = Left(val, Len(val) - 1)
        val = Trim(val)

        Select Case val
            Case "true"  : getJSONValue = True
            Case "false" : getJSONValue = False
            Case "null" : getJSONValue = Null
            Case Else
                If (Instr(val, """") = 0) Then
                    If IsNumeric(val) Then
                        getJSONValue = CDbl(val)
                    Else
                        getJSONValue = val
                    End If
                Else
                    If Left(val,1) = """" Then val = Mid(val, 2)
                    If Right(val,1) = """" Then val = Left(val, Len(val) - 1)
                    getJSONValue = aj_JSONDecode(Trim(val))
                End If
        End Select
    End Function

    Private JSONoutput_level
    Public Function JSONoutput()
        JSONoutput_level = 1
        wrap_dicttype = "[]"
        For Each aj_label In data
             If Not aj_IsInt(aj_label) Then wrap_dicttype = "{}"
        Next
        JSONoutput = Left(wrap_dicttype, 1) & Chr(13) & Chr(10) & GetDict(data) & Right(wrap_dicttype, 1)
    End Function

    Private Function GetDict(objDict)
        dim aj_item, aj_keyvals, aj_label, aj_dicttype
        For Each aj_item In objDict
            Select Case TypeName(objDict.Item(aj_item))
                Case "Dictionary"
                    GetDict = GetDict & Space(JSONoutput_level * 4)

                    aj_dicttype = "[]"
                    For Each aj_label In objDict.Item(aj_item).Keys
                         If Not aj_IsInt(aj_label) Then aj_dicttype = "{}"
                    Next
                    If aj_IsInt(aj_item) Then
                        GetDict = GetDict & (Left(aj_dicttype,1) & Chr(13) & Chr(10))
                    Else
                        GetDict = GetDict & ("""" & aj_JSONEncode(aj_item) & """" & ": " & Left(aj_dicttype,1) & Chr(13) & Chr(10))
                    End If
                    JSONoutput_level = JSONoutput_level + 1

                    aj_keyvals = objDict.Keys
                    GetDict = GetDict & (GetSubDict(objDict.Item(aj_item)) & Space(JSONoutput_level * 4) & Right(aj_dicttype,1) & aj_InlineIf(aj_item = aj_keyvals(objDict.Count - 1),"" , ",") & Chr(13) & Chr(10))
                Case Else
                    aj_keyvals =  objDict.Keys
                    GetDict = GetDict & (Space(JSONoutput_level * 4) & aj_InlineIf(aj_IsInt(aj_item), "", """" & aj_JSONEncode(aj_item) & """: ") & WriteValue(objDict.Item(aj_item)) & aj_InlineIf(aj_item = aj_keyvals(objDict.Count - 1),"" , ",") & Chr(13) & Chr(10))
            End Select
        Next
    End Function

    Private Function aj_IsInt(val)
        aj_IsInt = (TypeName(val) = "Integer" Or TypeName(val) = "Long")
    End Function

    Private Function GetSubDict(objSubDict)
        GetSubDict = GetDict(objSubDict)
        JSONoutput_level= JSONoutput_level -1
    End Function

    Private Function WriteValue(ByVal val)
        Select Case TypeName(val)
            Case "Double", "Integer", "Long": WriteValue = val
            Case "Null"                     : WriteValue = "null"
            Case "Boolean"                  : WriteValue = aj_InlineIf(val, "true", "false")
            Case Else                       : WriteValue = """" & aj_JSONEncode(val) & """"
        End Select
    End Function

    Private Function aj_JSONEncode(ByVal val)
        val = Replace(val, "\", "\\")
        val = Replace(val, """", "\""")
        'val = Replace(val, "/", "\/")
        val = Replace(val, Chr(8), "\b")
        val = Replace(val, Chr(12), "\f")
        val = Replace(val, Chr(10), "\n")
        val = Replace(val, Chr(13), "\r")
        val = Replace(val, Chr(9), "\t")
        aj_JSONEncode = Trim(val)
    End Function

    Private Function aj_JSONDecode(ByVal val)
        val = Replace(val, "\""", """")
        val = Replace(val, "\\", "\")
        val = Replace(val, "\/", "/")
        val = Replace(val, "\b", Chr(8))
        val = Replace(val, "\f", Chr(12))
        val = Replace(val, "\n", Chr(10))
        val = Replace(val, "\r", Chr(13))
        val = Replace(val, "\t", Chr(9))
        aj_JSONDecode = Trim(val)
    End Function

    Private Function aj_InlineIf(condition, returntrue, returnfalse)
        If condition Then aj_InlineIf = returntrue Else aj_InlineIf = returnfalse
    End Function

    Private Function aj_Strip(ByVal val, stripper)
        If Left(val, 1) = stripper Then val = Mid(val, 2)
        If Right(val, 1) = stripper Then val = Left(val, Len(val) - 1)
        aj_Strip = val
    End Function

    Private Function aj_MultilineTrim(TextData)
        aj_MultilineTrim = aj_RegExp.Replace(TextData, "$1")
    End Function

    private function aj_Trim(val)
        aj_Trim = Trim(val)
        Do While Left(aj_Trim, 1) = Chr(9) : aj_Trim = Mid(aj_Trim, 2) : Loop
        Do While Right(aj_Trim, 1) = Chr(9) : aj_Trim = Left(aj_Trim, Len(aj_Trim) - 1) : Loop
        aj_Trim = Trim(aj_Trim)
    end function
End Class
%>
```
