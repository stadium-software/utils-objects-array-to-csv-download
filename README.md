# List To CSV Download

Reformats a *List* to CSV and downloads a file with the CSV data to the client machine. 

Please be aware that retreiving large amounts of data from a database or API can cause the browser to hang or crash. Similarily, passing large amounts of data to the script below may also cause the browser to hang or crash. 

## Version 

1.0 Initial

1.1 Added optional script parameter "FileName"

1.2 Added optional script parameter "Separator"; changed "AddDoubleQuotes" input to flexible "ValueWrapper" (optional)

# Global Script Setup
1. Create a Global Script called "ListToCSVDownload"
2. Add the input parameters below to the Global Script
   1. List
   2. ValueWrapper
   3. FileName
   4. Separator
3. Drag a *JavaScript* action into the script
4. Add the Javascript below into the JavaScript code property
```javascript
/* Stadium Script v1.2 https://github.com/stadium-software/utils-list-to-csv-download */
let items = ~.Parameters.Input.List;
let fileName = ~.Parameters.Input.FileName || 'download.csv';
let wrap = ~.Parameters.Input.ValueWrapper || "";
let separator = ~.Parameters.Input.Separator || ",";

const download = (data) => {
    const blob = new Blob([data], { type: "text/csv" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = fileName;
    a.click();
};
const csvmaker = () => {
    let ret;
    if (items.length > 0 && typeof items[0] !== "object") {
        ret = Object.values(items).join(",");
    } else if (items.length > 0) {
        let rows = [],
            headers = [];
        for (let i = 0; i < items.length; i++) {
            headers = headers.concat(Object.keys(items[i]));
        }
        headers = Array.from(new Set(headers));
        for (let i = 0; i < items.length; i++) {
            let ob = headers.reduce((acc, curr) => ((acc[curr] = ""), acc), {});
            let obj = Object.assign(ob, items[i]);
            let values = Object.values(obj);
            rows.push(wrap + values.join(wrap + separator + wrap) + wrap);
        }
        headers = Array.from(new Set(headers));
        ret = [wrap + headers.join(wrap + separator + wrap) + wrap, rows.join("\n")].join("\n");
    }
    return ret;
};
const get = async () => {
    const csvdata = csvmaker();
    download(csvdata, fileName);
};
get();
```

## Event Handler
1. Drag the "ListToCSVDownload" script into an event handler or script
2. Provide values for the script input parameters
   1. List: Assign a *List* or a JavaScript array (e.g. Stadium List, database query results, JSON, etc.)
   2. ValueWrapper (optional): If you want to wrap the values, provide a wrapping character (e.g. a quote ' or a double quote ")
   3. FileName (optional): Default filename is 'download.csv'
   4. Separator (optional): Default separator is a comma

**Example data**
```json
[{
	"firstname": "George",
	"lastname": "Harrison",
	"band": "Beatles",
	"instrument": "guitar",
	"living": "false"
},{
	"firstname": "John",
	"lastname": "Lennon",
	"band": "Beatles",
	"instrument": "guitar",
	"living": "false"
},{
	"firstname": "Paul",
	"lastname": "McCartney",
	"band": "Beatles",
	"instrument": "bass",
	"living": "true"
},{
	"firstname": "Ringo",
	"lastname": "Star",
	"band": "Beatles",
	"instrument": "drums",
	"living": "true"
},{
	"firstname": "David",
	"lastname": "Gilmour",
	"band": "Pink Floyd",
	"instrument": "guitar",
	"living": "true"
},{
	"firstname": "Roger",
	"lastname": "Waters",
	"band": "Pink Floyd",
	"instrument": "bass",
	"living": "true"
},{
	"firstname": "Syd",
	"lastname": "Barrett",
	"band": "Pink Floyd",
	"instrument": "guitar",
	"living": "false"
},{
	"firstname": "Bob",
	"lastname": "Marley",
	"band": "The Wailers",
	"instrument": "voice",
	"living": "false"
},{
	"firstname": "Pete",
	"lastname": "Townshend",
	"band": "The Who",
	"instrument": "guitar",
	"living": "true"
},{
	"firstname": "Freddie",
	"lastname": "Mercury",
	"band": "Queen",
	"instrument": "voice",
	"living": "false"
},{
	"firstname": "Brian",
	"lastname": "May",
	"band": "Queen",
	"instrument": "guitar",
	"living": "false"
},{
	"firstname": "Jim",
	"lastname": "Morrison",
	"band": "The Doors",
	"instrument": "voice",
	"living": "false"
},{
	"firstname": "Jimi",
	"lastname": "Hendrix",
	"band": "The Jimi Hendrix Experience",
	"instrument": "guitar",
	"living": "false"
},{
	"firstname": "Michael",
	"lastname": "Jackson",
	"band": "Jackson 5",
	"instrument": "voice",
	"living": "false"
},{
	"firstname": "Rogers",
	"lastname": "Nelson",
	"band": "the Revolution",
	"instrument": "guitar",
	"living": "false"
}]
```

**Converted**
<pre>
firstname,lastname,band,instrument,living
George,Harrison,Beatles,guitar,false
John,Lennon,Beatles,guitar,false
Paul,McCartney,Beatles,bass,true
Ringo,Star,Beatles,drums,true
David,Gilmour,Pink Floyd,guitar,true
Roger,Waters,Pink Floyd,bass,true
Syd,Barrett,Pink Floyd,guitar,false
Bob,Marley,The Wailers,voice,false
Pete,Townshend,The Who,guitar,true
Freddie,Mercury,Queen,voice,false
Brian,May,Queen,guitar,false
Jim,Morrison,The Doors,voice,false
Jimi,Hendrix,The Jimi Hendrix Experience,guitar,false
Michael,Jackson,Jackson 5,voice,false
Rogers,Nelson,the Revolution,guitar,false
</pre>