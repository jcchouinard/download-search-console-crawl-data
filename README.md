This is the bookmarklet: <a href="javascript:(function()%7Bconst%20crawlGraphs%20%3D%20document.querySelectorAll(%22.hostload-activity%20script%22)%3B%0A%0Aconst%20isSafari%20%3D%20()%20%3D%3E%0A%09%2F%5E((%3F!chrome%7Candroid).)*safari%2Fi.test(navigator.userAgent)%3B%0A%0Aconst%20toCSV%20%3D%20(data%2C%20headers%2C%20separator%2C%20enclosingCharacter)%20%3D%3E%20%7B%0A%09return%20jsons2csv(data%2C%20headers%2C%20separator%2C%20enclosingCharacter)%3B%0A%7D%3B%0A%0Aconst%20isJsons%20%3D%20array%20%3D%3E%0A%09Array.isArray(array)%20%26%26%0A%09array.every(row%20%3D%3E%20typeof%20row%20%3D%3D%3D%20%22object%22%20%26%26%20!(row%20instanceof%20Array))%3B%0A%0Aconst%20jsons2arrays%20%3D%20(jsons%2C%20headers)%20%3D%3E%20%7B%0A%09headers%20%3D%20headers%20%7C%7C%20jsonsHeaders(jsons)%3B%0A%0A%09%2F%2F%20allow%20headers%20to%20have%20custom%20labels%2C%20defaulting%20to%20having%20the%20header%20data%20key%20be%20the%20label%0A%09let%20headerLabels%20%3D%20headers%3B%0A%09let%20headerKeys%20%3D%20headers%3B%0A%09if%20(isJsons(headers))%20%7B%0A%09%09headerLabels%20%3D%20headers.map(header%20%3D%3E%20header.label)%3B%0A%09%09headerKeys%20%3D%20headers.map(header%20%3D%3E%20header.key)%3B%0A%09%7D%0A%0A%09console.log(headerLabels)%3B%0A%09console.log(headerKeys)%3B%0A%0A%09const%20data%20%3D%20jsons.map(object%20%3D%3E%0A%09%09headerKeys.map(header%20%3D%3E%20getHeaderValue(header%2C%20object))%0A%09)%3B%0A%0A%09return%20%5BheaderLabels%2C%20...data%5D%3B%0A%7D%3B%0A%0Aconst%20jsonsHeaders%20%3D%20array%20%3D%3E%0A%09Array.from(%0A%09%09array%0A%09%09%09.map(json%20%3D%3E%20Object.keys(json))%0A%09%09%09.reduce((a%2C%20b)%20%3D%3E%20new%20Set(%5B...a%2C%20...b%5D)%2C%20%5B%5D)%0A%09)%3B%0A%0Aconst%20getHeaderValue%20%3D%20(property%2C%20obj)%20%3D%3E%20%7B%0A%09const%20foundValue%20%3D%20property%0A%09%09.replace(%2F%5C%5B(%5B%5E%5C%5D%5D%2B)%5D%2Fg%2C%20%22.%241%22)%0A%09%09.split(%22.%22)%0A%09%09.reduce(function(o%2C%20p%2C%20i%2C%20arr)%20%7B%0A%09%09%09%2F%2F%20if%20at%20any%20point%20the%20nested%20keys%20passed%20do%20not%20exist%2C%20splice%20the%20array%20so%20it%20doesnt%20keep%20reducing%0A%09%09%09if%20(o%5Bp%5D%20%3D%3D%3D%20undefined)%20%7B%0A%09%09%09%09arr.splice(1)%3B%0A%09%09%09%7D%20else%20%7B%0A%09%09%09%09return%20o%5Bp%5D%3B%0A%09%09%09%7D%0A%09%09%7D%2C%20obj)%3B%0A%0A%09return%20foundValue%20%3D%3D%3D%20undefined%20%3F%20%22%22%20%3A%20foundValue%3B%0A%7D%3B%0A%0Aconst%20jsons2csv%20%3D%20(data%2C%20headers%2C%20separator%2C%20enclosingCharacter)%20%3D%3E%0A%09joiner(jsons2arrays(data%2C%20headers)%2C%20separator%2C%20enclosingCharacter)%3B%0A%0Aconst%20joiner%20%3D%20(data%2C%20separator%20%3D%20%22%2C%22%2C%20enclosingCharacter%20%3D%20'%22')%20%3D%3E%20%7B%0A%09const%20output%20%3D%20data%0A%09%09.filter(e%20%3D%3E%20e)%0A%09%09.map(row%20%3D%3E%0A%09%09%09row%0A%09%09%09%09.map(element%20%3D%3E%20elementOrEmpty(element))%0A%09%09%09%09.map(%0A%09%09%09%09%09column%20%3D%3E%0A%09%09%09%09%09%09%60%24%7BenclosingCharacter%7D%24%7Bcolumn%7D%24%7BenclosingCharacter%7D%60%0A%09%09%09%09)%0A%09%09%09%09.join(separator)%0A%09%09)%0A%09%09.join(%60%5Cn%60)%3B%0A%0A%09return%20output%3B%0A%7D%3B%0A%0Aconst%20elementOrEmpty%20%3D%20element%20%3D%3E%20(element%20%7C%7C%20element%20%3D%3D%3D%200%20%3F%20element%20%3A%20%22%22)%3B%0A%0Aconst%20buildURI%20%3D%20(data%2C%20uFEFF%2C%20headers%2C%20separator%2C%20enclosingCharacter)%20%3D%3E%20%7B%0A%09const%20csv%20%3D%20toCSV(data%2C%20headers%2C%20separator%2C%20enclosingCharacter)%3B%0A%09const%20type%20%3D%20isSafari()%20%3F%20%22application%2Fcsv%22%20%3A%20%22text%2Fcsv%22%3B%0A%09const%20blob%20%3D%20new%20Blob(%5BuFEFF%20%3F%20%22%5CuFEFF%22%20%3A%20%22%22%2C%20csv%5D%2C%20%7B%20type%20%7D)%3B%0A%09const%20dataURI%20%3D%20%60data%3A%24%7Btype%7D%3Bcharset%3Dutf-8%2C%24%7BuFEFF%20%3F%20%22%5CuFEFF%22%20%3A%20%22%22%7D%24%7Bcsv%7D%60%3B%0A%0A%09return%20dataURI%3B%0A%7D%3B%0A%0A%2F%2F%20This%20texts%20the%20script%20tag%20from%20search%20console%0Afunction%20extractJSONDataFromGraphs(chartData)%20%7B%0A%09const%20extractJSON%20%3D%20new%20RegExp(%2FsetData%5C((%5B%5E%3B%5D%2B)%5C)%3B%2F%2C%20%22i%22)%3B%0A%0A%09%2F%2F%20Extract%20the%20JSON%20from%20the%20string%0A%09const%20jsonStr%20%3D%20chartData.match(extractJSON)%5B1%5D%3B%0A%09%2F%2F%20const%20jsonStr%20%3D%20regexObj%5B1%5D%3B%0A%0A%09%2F%2F%20There%20are%20a%20bunch%20of%20raw%20dates%20in%20the%20output.%20Remove%20them.%0A%09const%20jsonStrSanitize%20%3D%20jsonStr.replace(%0A%09%09%2Fnew%20Date%5C(%5Cd%2B%2C%5Cd%2B%2C%5Cd%2B%5C)%2Fgi%2C%0A%09%09'%22dateObj%22'%0A%09)%3B%0A%0A%09%2F%2F%20Sanitize%20the%20JSON%0A%09const%20graphJSON%20%3D%20JSON.parse(jsonStrSanitize)%3B%0A%0A%09const%20headers%20%3D%20graphJSON.cols.map(colData%20%3D%3E%20%7B%0A%09%09let%20colKey%20%3D%20%22%22%3B%0A%0A%09%09%2F%2F%20Map%20for%20CSV%20output%0A%09%09if%20(colData.label%20%3D%3D%3D%20%22Date%22)%20%7B%0A%09%09%09colKey%20%3D%20%22date%22%3B%0A%09%09%7D%20else%20%7B%0A%09%09%09colKey%20%3D%20%22data%22%3B%0A%09%09%7D%0A%0A%09%09return%20%7B%20key%3A%20colKey%2C%20label%3A%20colData.label%20%7D%3B%0A%09%7D)%3B%0A%0A%09const%20data%20%3D%20graphJSON.rows.map(rowData%20%3D%3E%20%7B%0A%09%09const%20date%20%3D%20rowData.c%5B0%5D.f%3B%0A%09%09const%20number%20%3D%20rowData.c%5B1%5D.v%3B%0A%0A%09%09return%20%7B%20date%3A%20date%2C%20data%3A%20number%20%7D%3B%0A%09%7D)%3B%0A%0A%09return%20%7B%0A%09%09headers%2C%0A%09%09data%0A%09%7D%3B%0A%7D%0A%0Agraphs%20%3D%20%5B%22pages_crawled_per_day.csv%22%2C%20%22kb_downloaded_per_day.csv%22%2C%22time_spent_downloading_page.csv%22%5D%0A%0A%2F%2F%20Loop%20over%20the%20graphs%0AcrawlGraphs.forEach((chart%2Ci)%20%3D%3E%20%7B%0A%09output%20%3D%20extractJSONDataFromGraphs(chart.text)%3B%0A%09dataURI%20%3D%20buildURI(output.data%2C%20%22uFEFF%22%2C%20output.headers%2C%20%22%2C%22%2C%20'%22')%3B%0A%0A%09var%20link%20%3D%20document.createElement(%22a%22)%3B%0A%09link.setAttribute(%22href%22%2C%20dataURI)%3B%0A%09link.setAttribute(%22download%22%2C%20graphs%5Bi%5D)%3B%0A%09document.body.appendChild(link)%3B%20%2F%2F%20Required%20for%20FF%0A%0A%09link.click()%3B%0A%7D)%3B%7D)()%3B">bookmarklet</a>
