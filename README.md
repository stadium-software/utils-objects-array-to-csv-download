# List To CSV Download

Reformats a *List* to CSV and downloads a file with the CSV data to the client machine. 

## Version 

1.0 Initial

# Global Script Setup
1. Create a Global Script called "ListToCSVDownload"
2. Add the input parameters below to the Global Script
   1. List
   2. AddDoubleQuotes
3. Drag a *JavaScript* action into the script
4. Add the Javascript below into the JavaScript code property
```javascript
/* Stadium Script v1.0 https://github.com/stadium-software/utils-list-to-csv-download */
const download = (data) => {
    const blob = new Blob([data], { type: 'text/csv' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'download.csv';
    a.click();
};
const csvmaker = (data, qt) => {
    let ret;
    if (data.length > 0 && typeof data[0] !== "object") {
        ret = Object.values(data).join(',');
    } else if (data.length > 0) {
        let rows = [], headers = [];
        for (let i = 0; i < data.length; i++) {
            headers = headers.concat(Object.keys(data[i]));
        }
        headers = Array.from(new Set(headers));
        for (let i = 0; i < data.length; i++) {
            let ob = headers.reduce((acc, curr) => ((acc[curr] = ""), acc), {});
            let obj = Object.assign(ob, data[i]);
            let values = Object.values(obj);
            if (qt) {
                rows.push('"' + values.join('","') + '"');
            } else { 
                rows.push(values.join(','));
            }
        }
        headers = Array.from(new Set(headers));
        ret = [headers.join(','), rows.join('\n')].join('\n');
    }
    return ret;
};
const get = async (data, wrapper) => {
    const csvdata = csvmaker(data, wrapper);
    download(csvdata);
};
const items = ~.Parameters.Input.List;
const wrap = ~.Parameters.Input.AddDoubleQuotes;
get(items, wrap);
```

## Event Handler
1. Drag the "ListToCSVDownload" script into an event handler or script
2. Provide values for the script input parameters
   1. List: Assign a *List* or a JavaScript array (e.g. database query results, JSON, etc.)
   2. AddDoubleQuotes (optional): Add true to wrap values in double quotes (default is false)

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