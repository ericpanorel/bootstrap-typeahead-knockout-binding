bootstrap-typeahead-knockout-binding
====================================

KnockoutJS Binding handler  for Bootstrap TypeAhead plugin

<h2>Sample Usage</h2>
<p>HTML View</p>
 ```
    <form class="form-search form-horizontal">
        <div class="control-group">
            <label class="control-label">
                Enter your text
            </label>
            <div class="controls">
                <input type="text" class="search-query required" name="YourText" id="YourText" placeholder="Search" data-bind="TypeAhead: { source: Suggestions, minLength: 2 }, value: YourText">
            </div>
        </div>
    </form>
 ```
 <p>Javascript View Model & Code</p>
 ```
         (function (ko, $) {
            "use strict";
            var my = my || {};

            //ViewModel
            my.vm = (function () {
                var YourText = ko.observable(),
                    Suggestions = function (searchTerm, callback) {


                        $.ajax({
                            type: "POST",
                            url: '/api/search',
                            data: { Term: searchTerm },
                            dataType: 'json',
                            success: function (data) {

                                callback(data);
                            },
                            error: function () {
                                callback(['']);

                            }
                        });

                    };

                return {
                    Suggestions: Suggestions,
                    YourText: YourText
                }

            })();


            // document ready
            $(document).ready(function () {
                ko.applyBindings(my.vm);
            });


        })(ko, jQuery);
 ```
<p>ASP .Net API Controller.</p>
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Web.Http;

namespace MvcApplication1.Controllers
{
    public class SearchParam
    {
        public string Term { get; set; }
    }
    public class SearchController : ApiController
    {
        public IEnumerable<string> Post(SearchParam data)
        {
            // Assemble your data here        
            return new string[] {"John","Mark","Joseph" };
        }
    }
}
```
<p>Note: This works with other back-end technologies too, like PhP, classic ASP, etc...</p>
