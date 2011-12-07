## DataTableFooter Plugin for YUI 3

Includes support for DataTableScroll and use of "fixed" (i.e. non-scrolling") footer.

Currently available "macros" are {SUM}, {AVG}, {MIN}, {MAX}, {ROW\_COUNT}, {COL\_COUNT}, {DATE}, {TIME}.

In the near future we'll be providing a custom function capability (via fnValue) within the footer cells .

A working example is available at [dt_footer_plugin_use.html](http://www.blunderalong.com/pub/yui3/dt_footer_plugin_use.html)

An image of a basic DT with footer is ![](dt_plugin_basic.jpg)

### Including the Plugin

Include the source code for the plugin on your page as;  
`<script type="text/javascript" src="dt_foot_plugin.js"></script>`

And the CSS file;  
`<link href="dt_foot_plugin.css" rel="stylesheet" type="text/css" />`

(this has not be contributed to the YUI Gallery yet ... sorry)

### Using the Plugin & Configuration

You tell the YUI Loader about this plugin as  
`YUI().use( "datatable-base", ..... your other stuff ...., "datatable-footer", function(Y) {`

Within your code you attach the plugin to your DataTable instance as a typical YUI Plugin;

`myDT.plug( Y.Plugin.DataTableFooter, { ... configs ...} );`

Where the current configs are;

* heading {Object}  with members,
 * colspan {Integer} :  No. of columns to merge to make TH element (begins at 1st column always!)
 * label   {String}  :  Label to be content of TH, can include tags {ROW\_COUNT}, {COL\_COUNT}, {DATE}, {TIME}
 * className {String}:  Additional CSS classes to attach to the TH Liner element
* fixed {Boolean} :     Flag to indicate if footer should be "fixed" or "floating"
* col_keys {Array} of Column objects ...
 * where each column object contains
  * key {String} : Datatable "key" name ... tells the horizontal position of this footer element
  * calc {String} :  A summary calculation string ... any of **{SUM},{AVG},{MIN},{MAX},{ROW\_COUNT},{COL\_COUNT},{DATE},{TIME}**
  * formatter {Function} : DataTable formatter function to use. if none provided, will use the DT's Column formatter or none.
  * className {String} : Additional CSS class to attach to the TD liner element. (includes 'align-right', 'align-center')
  * fnValue {Function} : Function value for insertion into TD liner (FUTURE!)

### An Example

The code for the image example from above would look like this;  
(For a basic DataTable instance with columns 


<code>
    var fmtTotal = function(o) {
		  return Y.DataType.Number.format( o.value, { prefix:"$ ", thousandsSeparator:",", decimalPlaces:2 } );
    }
    
    var jsCustomers = [
		  { cust_id:1, cust_name:"George Simon", location:"New York", status:1, no_orders:3, ord_total:8546.8 },
		  { cust_id:2, cust_name:"Angela Marconi", location:"Florence, Italy", status:1, no_orders:1, ord_total:8733.04 },
		  { cust_id:3, cust_name:"Nancy Keeler", location:"Chicago", status:2, no_orders:2, ord_total:1784.32 },
		  { cust_id:4, cust_name:"Mickey Gates", location:"Seattle", status:1, no_orders:7, ord_total:7209.31 },
		  { cust_id:5, cust_name:"Wuchan Hsu", location:"Beijing", status:3, no_orders:0, ord_total:0 },
		  { cust_id:6, cust_name:"Dmitri Zaganuv", location:"Russia", status:2, no_orders:1, ord_total:907.10 } 
	  ];
			  	
    var cols = [ 
	  	{ key:"cust_id", label:"Cust ID" }, 
		  { key:"cust_name", label:"Customer" },
		  { key:"location", label:"Location" },
		  { key:"no_orders", label:"No. Orders" },
		  { key:"ord_total", label:"Order Total", formatter:fmtTotal } 
	  ];

    var dt = new Y.DataTable.Base( { 
      columnset: cols,
      recordset: jsCustomers
    });

    dt.plug( Y.Plugin.DataTableFooter, {
  		fixed : true,
  		heading : { colspan : 3,  label:"TOTALS ( {ROW_COUNT} Customers) :", className:"align-right" },
  		col_keys : [
  		   { key:"no_orders",  calc:"{SUM}" },
  		   { key:"ord_total",  calc:"{SUM}"}
  		]
  	});

  	dt.render("#dtable");
</code>
  
The full working example of this code is at <http://www.blunderalong.com/pub/yui3/dt_foot_plugin_basic.html>  


### References

YUI contributor Matt Parker developed a "Table footer statistics for YUI 2 DataTable" extension that was 
documented in his YUIBlog article of January 13, 2011 (see <http://www.yuiblog.com/blog/2011/01/13/table-footer-statistics-for-yui-2-datatable/>).  

Parker's extension supports a number of mathematical functions but was limited to only working on a non-scrollable DataTable.  I also found 
in my use of his extension that most of the time I simply needed a column "sum", not standard deviations, etc...

Additionally, Satyam provided has provided a very straightforward technique for constructing the TFOOT DOM elements (see <http://www.satyam.com.ar/yui/2.6.0/invoice.html>).

These resources were very informative in developing this Plugin, Thanks Matt and Satyam!
