# List To CSV Download

Reformats a *List* to CSV and downloads a file with the CSV data to the client machine. 

## Version 

1.0 Initial

# Global Script Setup
1. Create a Global Script called "ListToCSVDownload"
2. Add the input parameters below to the Global Script
   1. List
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
const csvmaker = (data) => {
    let rows = [], ret;
    const headers = Object.keys(data[0]);
    if (headers[0] == 0) {
        ret = Object.values(data).join(',');
    } else {
        for (let i = 0; i < data.length; i++) {
            let values = Object.values(data[i]);
            rows.push(values.join(','));
        }
        ret = [headers.join(','), rows.join('\n')].join('\n');
    }
    return ret;
};
const get = async (data) => {
    const csvdata = csvmaker(data);
    download(csvdata);
};
const items = ~.Parameters.Input.List;
get(items);
```

## Event Handler
1. Drag the "ListToCSVDownload" script into an event handler or script
2. Assign a List (such as database queries or JSON from an API call) to the "List" input parameter

*Example data*
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
},{
	"firstname": "Whitney",
	"lastname": "Houston",
	"instrument": "voice",
	"living": "false"
}]
```
