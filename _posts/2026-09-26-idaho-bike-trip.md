---
layout: post
title:  "Bikepacking the Idaho Backcountry: McCall, Big Creek, Warren, and Burgdorf"
date:    2026-09-26 00:00:00 -0700
categories: outdoors
---

In late September, a group of six of us (Dan, Jordan, Boris, Hazel, Rylan, and I) flew to Boise for a three-day gravel bikepacking loop through the mountains of central Idaho. Starting and ending in McCall, we rode remote dirt roads and singletrack through Yellow Pine, Big Creek, Warren, and Burgdorf Hot Springs, crossing summits over 8,600 ft and staying in backcountry lodges every night. It was beautiful, cold, steep, rocky, occasionally miserable, and the most intense physical activity I've ever done.

![Group photo at the end in McCall]({{site.baseurl}}/assets/img/idaho/web/mccall-finish.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Tired but happy, back in McCall after three days of riding.
</span>

## The Route

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

<style>.place-label { font-weight: 600; font-size: 12px; padding: 2px 6px; }</style>
<div id="route-map" style="height: 500px; width: 100%; border-radius: 4px;"></div>
<span style="display: flex;justify-content: center;font-style:italic;">
Our route over three days. Click a dot to see the photo taken there.
</span>

<script>
(function () {
  var base = "{{site.baseurl}}/assets/img/idaho/web/";
  var routeDir = "{{site.baseurl}}/assets/img/idaho/routes/";
  var dayColors = { 1: "#d7301f", 2: "#2166ac", 3: "#1a9850" };

  // Geotagged photos, in chronological order within each day.
  var photos = [
    { day: 1, lat: 44.9205, lon: -115.9618, img: "warren-wagon-road.jpg", caption: "Warren Wagon Road out of McCall" },
    { day: 1, lat: 44.9281, lon: -115.9473, img: "cold-start-selfie.jpg", caption: "A chilly 35&deg;F start" },
    { day: 1, lat: 45.0629, lon: -115.7600, img: "bridge-break.jpg", caption: "Bridge break on Lick Creek Road" },
    { day: 1, lat: 44.9648, lon: -115.4932, img: "yellow-pine-group.jpg", caption: "Yellow Pine" },
    { day: 1, lat: 44.9654, lon: -115.4765, img: "profile-gap-creek.jpg", caption: "Starting the Profile Gap climb" },
    { day: 1, lat: 45.1274, lon: -115.3243, img: "big-creek-store.jpg", caption: "Big Creek Lodge" },
    { day: 2, lat: 45.1274, lon: -115.3243, img: "thermometer.jpg", caption: "30&deg;F morning at Big Creek" },
    { day: 2, lat: 45.1511, lon: -115.4232, img: "elk-summit.jpg", caption: "Elk Summit" },
    { day: 2, lat: 45.1512, lon: -115.5831, img: "elk-descent-poster.jpg", caption: "Descending from Elk Summit" },
    { day: 2, lat: 45.1828, lon: -115.5698, img: "salmon-climb-poster.jpg", caption: "Starting the 4,000 ft climb" },
    { day: 2, lat: 45.1870, lon: -115.5703, img: "salmon-climb-switchbacks-poster.jpg", caption: "Switchbacks above the South Fork" },
    { day: 2, lat: 45.2300, lon: -115.6304, img: "warren-summit-climb.jpg", caption: "Near Warren Summit" },
    { day: 2, lat: 45.2771, lon: -115.9121, img: "burgdorf-pool.jpg", caption: "Burgdorf Hot Springs" },
    { day: 3, lat: 45.2771, lon: -115.9121, img: "burgdorf-woodshed.jpg", caption: "Bikes at Burgdorf" },
    { day: 3, lat: 45.1702, lon: -115.8126, img: "lick-creek-hike-a-bike.jpg", caption: "Hike-a-bike on Lick Creek trail" },
    { day: 3, lat: 45.2046, lon: -115.8131, img: "secesh-singletrack.jpg", caption: "Singletrack along the Secesh River" },
    { day: 3, lat: 45.2506, lon: -115.8173, img: "burgdorf-road.jpg", caption: "Gravel back to Burgdorf" },
    { day: 3, lat: 45.1448, lon: -116.0136, img: "secesh-descent-poster.jpg", caption: "The 25 mile descent to McCall" },
    { day: 3, lat: 44.9092, lon: -116.0994, img: "mccall-finish.jpg", caption: "Back in McCall" }
  ];

  var map = L.map("route-map", { scrollWheelZoom: false });
  var esriAttr = "Tiles &copy; Esri";
  var osmAttr = '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors';
  var esriUrl = function (service) {
    return "https://server.arcgisonline.com/ArcGIS/rest/services/" + service + "/MapServer/tile/{z}/{y}/{x}";
  };
  var baseLayers = {
    "Topo": L.tileLayer(esriUrl("World_Topo_Map"), { maxZoom: 18, attribution: esriAttr + " &mdash; Esri, USGS, NOAA" }),
    "Satellite": L.layerGroup([
      L.tileLayer(esriUrl("World_Imagery"), { maxZoom: 18, attribution: esriAttr + " &mdash; Esri, Maxar, Earthstar Geographics" }),
      L.tileLayer(esriUrl("Reference/World_Boundaries_and_Places"), { maxZoom: 18 })
    ]),
    "Shaded relief": L.layerGroup([
      L.tileLayer(esriUrl("World_Shaded_Relief"), { maxZoom: 13, attribution: esriAttr + " &mdash; Esri" }),
      L.tileLayer(esriUrl("Reference/World_Boundaries_and_Places"), { maxZoom: 13 })
    ]),
    "Streets": L.tileLayer("https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}.png", {
      maxZoom: 19, attribution: osmAttr + ' &copy; <a href="https://carto.com/attributions">CARTO</a>'
    }),
    "Light": L.tileLayer("https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png", {
      maxZoom: 19, attribution: osmAttr + ' &copy; <a href="https://carto.com/attributions">CARTO</a>'
    }),
    "OpenTopoMap": L.tileLayer("https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png", {
      maxZoom: 17, attribution: osmAttr + ', SRTM | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (CC-BY-SA)'
    })
  };
  baseLayers["Topo"].addTo(map);
  L.control.layers(baseLayers, null, { position: "topright" }).addTo(map);

  // Keep photo dots above the GPS tracks and place pins so they stay clickable.
  map.createPane("photos").style.zIndex = 625;

  var places = [
    { lat: 44.9092, lon: -116.0994, name: "McCall", note: "Start and finish", dir: "left" },
    { lat: 44.9650, lon: -115.4924, name: "The Corner, Yellow Pine", note: "Day 1 lunch", dir: "bottom" },
    { lat: 45.1273, lon: -115.3245, name: "Big Creek Lodge", note: "Night 1", dir: "left" },
    { lat: 45.2665, lon: -115.6808, name: "Warren", note: "Day 2 lunch at the Baum Shelter", dir: "top" },
    { lat: 45.2485, lon: -115.8170, name: "Yeti Backcountry Pizza", note: "Day 3 lunch", dir: "bottom" },
    { lat: 45.2770, lon: -115.9121, name: "Burgdorf Hot Springs", note: "Night 2", dir: "top" }
  ];
  places.forEach(function (p) {
    L.marker([p.lat, p.lon], { title: p.name })
      .bindTooltip(p.name, { permanent: true, direction: p.dir, className: "place-label" })
      .bindPopup("<b>" + p.name + "</b><br>" + p.note)
      .addTo(map);
  });

  var bounds = L.latLngBounds([]);
  photos.forEach(function (p) {
    var ll = [p.lat, p.lon];
    bounds.extend(ll);
    L.circleMarker(ll, { pane: "photos", radius: 6, color: "#fff", weight: 2, fillColor: dayColors[p.day], fillOpacity: 1 })
      .bindPopup('<img src="' + base + p.img + '" style="width:200px"><br><b>Day ' + p.day + ":</b> " + p.caption)
      .addTo(map);
  });
  map.fitBounds(bounds, { padding: [30, 30] });
  window.addEventListener("load", function () {
    map.invalidateSize();
    map.fitBounds(bounds, { padding: [30, 30] });
  });

  // Approximate route connecting the photo locations, replaced by GPS tracks when available.
  var fallback = {};
  [1, 2, 3].forEach(function (day) {
    var pts = photos.filter(function (p) { return p.day === day; }).map(function (p) { return [p.lat, p.lon]; });
    fallback[day] = L.polyline(pts, { color: dayColors[day], weight: 3, dashArray: "6 6", opacity: 0.8 }).addTo(map);
  });

  // Strava GPX exports: routes/day1.gpx, routes/day2.gpx, routes/day3.gpx
  [1, 2, 3].forEach(function (day) {
    fetch(routeDir + "day" + day + ".gpx").then(function (r) {
      if (!r.ok) { return; }
      return r.text().then(function (txt) {
        var xml = new DOMParser().parseFromString(txt, "application/xml");
        var pts = Array.prototype.map.call(xml.getElementsByTagName("trkpt"), function (pt) {
          return [parseFloat(pt.getAttribute("lat")), parseFloat(pt.getAttribute("lon"))];
        });
        if (!pts.length) { return; }
        map.removeLayer(fallback[day]);
        var line = L.polyline(pts, { color: dayColors[day], weight: 4, opacity: 0.9 }).addTo(map);
        line.bindTooltip("Day " + day, { sticky: true });
        bounds.extend(line.getBounds());
        map.fitBounds(bounds, { padding: [30, 30] });
      });
    });
  });

  var legend = L.control({ position: "bottomright" });
  legend.onAdd = function () {
    var div = L.DomUtil.create("div");
    div.style.cssText = "background:white;padding:6px 10px;border-radius:4px;font-size:13px;line-height:1.6";
    div.innerHTML = [1, 2, 3].map(function (d) {
      return '<span style="display:inline-block;width:14px;height:4px;background:' + dayColors[d] + ';vertical-align:middle;margin-right:6px"></span>Day ' + d;
    }).join("<br>");
    return div;
  };
  legend.addTo(map);
})();
</script>

- **[Day 1](https://www.strava.com/activities/20345092237):** McCall → Lick Creek Summit → Yellow Pine → Profile Gap (7,605 ft) → Big Creek Lodge<br>*74.3 mi · 6,486 ft climbing · 5,770 ft descent · 7:47:54 moving*
- **[Day 2](https://www.strava.com/activities/20369655872):** Big Creek → Elk Summit (8,670 ft) → South Fork Salmon River → Warren Summit → Warren → Burgdorf Hot Springs<br>*58.4 mi · 9,121 ft climbing · 8,710 ft descent · 8:30:51 moving*
- **[Day 3](https://www.strava.com/activities/20372115553):** Burgdorf → Loon Lake (almost) → Lick Creek & Secesh River trails → Yeti Backcountry Pizza → Burgdorf → Secesh Summit → McCall<br>*59.7 mi · 3,048 ft climbing · 4,130 ft descent · 5:55:26 moving*
- **Total:** *192.3 mi · 18,655 ft climbing · 18,610 ft descent · 22:14:11 moving*

## Day 0: Getting There

We flew from SFO to Boise on Friday, picked up our rental car, and drove north to McCall. After dinner at a local pizza place we crashed at our Airbnb to rest up for the next morning.

## Day 1: McCall to Big Creek

We were up at 6:30am and out the door by 7 for breakfast at the Foglifter Cafe. The food was "Jyght" based on our order blocks — as Jordan said, "we can do anything if we put our heads together!"

After parking the car in McCall, we geared up in the freezing cold. It was about 35°F, and I was very grateful to borrow Dan's extra gloves.

![Group selfie at the start in McCall]({{site.baseurl}}/assets/img/idaho/web/start-mccall.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Bundled up and ready to roll out of McCall.
</span>

We biked out along the lake on the Warren Wagon Road, which quickly turned to dirt and gravel. The morning sun on the fields and forests was beautiful, but the cold took its toll: Dan's feet got painfully cold, and Hazel had an issue with her shifter battery.

![Riding out on Warren Wagon Road]({{site.baseurl}}/assets/img/idaho/web/warren-wagon-road.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Beautiful morning light on the Warren Wagon Road.
</span>

![Selfie on a cold morning]({{site.baseurl}}/assets/img/idaho/web/cold-start-selfie.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
Cold, but stoked.
</span>

We turned onto Lick Creek Road and started the first big climb of the trip. At the top we stopped for snacks, and Rylan fixed an issue with her front wheel thru-axle.

![Riding through tall pines]({{site.baseurl}}/assets/img/idaho/web/tall-pines-road.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
Climbing through the pines.
</span>

![Nick on gravel in the morning]({{site.baseurl}}/assets/img/idaho/web/cold-morning-gravel.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
Still in full winter gear.
</span>

![Granite peaks near Lick Creek Summit]({{site.baseurl}}/assets/img/idaho/web/lick-creek-peaks.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Granite peaks near the top of Lick Creek Road.
</span>

Then came the first big, beautiful descent: perfect dirt and gravel road, ripping along at 30mph with sweeping views over the valley.

![Descending Lick Creek Road through fall colors]({{site.baseurl}}/assets/img/idaho/web/lick-creek-descent.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Fall colors on the Lick Creek descent.
</span>

![Rocky cliffs along the road]({{site.baseurl}}/assets/img/idaho/web/lick-creek-cliffs.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
Rocky cliffs along the descent.
</span>

We followed Lick Creek Road south toward the Salmon River and chilled on a bridge for a bit before continuing on a gentle uphill to Yellow Pine.

![Nick resting on a bridge]({{site.baseurl}}/assets/img/idaho/web/bridge-break.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
Bridge break.
</span>

![Riding along the river]({{site.baseurl}}/assets/img/idaho/web/river-road.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Cruising along the river toward Yellow Pine.
</span>

We rolled into Yellow Pine around 2pm and ate lunch at The Corner bar and grill, with lots of dogs roaming around freely. The steak fingers were terrible.

![Group photo in Yellow Pine]({{site.baseurl}}/assets/img/idaho/web/yellow-pine-group.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
The crew in Yellow Pine after lunch.
</span>

We left at 3:15pm for the big climb of the day: 3,000 ft up to Profile Gap (7,605 ft). Boris was really struggling on this climb and bonked hard, so Rylan and I hung back with him to chat and keep his spirits up.

![Riding into the Profile Gap climb]({{site.baseurl}}/assets/img/idaho/web/profile-gap-aspens.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
Aspens at the start of the Profile Gap climb.
</span>

![Riding along the creek]({{site.baseurl}}/assets/img/idaho/web/profile-gap-creek.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Climbing alongside the creek toward Profile Gap.
</span>

From the top we cruised downhill and arrived at Big Creek Lodge around 6:45pm, just in time for dinner.

Big Creek Lodge is beautiful! A huge, warm fire, friendly people, and very cool taxidermy, including a cougar pelt and a full black bear. Dinner was mid, but there was free tea and good vibes. I roomed with Jordan and paid for the worst cot I've ever slept on. After fixing my rear derailleur and oiling my chain, I was asleep by 9:45 with the help of a Unisom.

![Big Creek Lodge general store]({{site.baseurl}}/assets/img/idaho/web/big-creek-store.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Unpacking at Big Creek Lodge.
</span>

![Big Creek Lodge interior]({{site.baseurl}}/assets/img/idaho/web/big-creek-lodge.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
The main room and fireplace at Big Creek Lodge.
</span>

![Cougar pelt]({{site.baseurl}}/assets/img/idaho/web/cougar-pelt.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
The cougar pelt on the balcony railing.
</span>

![Taxidermy black bear]({{site.baseurl}}/assets/img/idaho/web/black-bear.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Making friends with the lodge's black bear.
</span>

## Day 2: Big Creek to Burgdorf

This was going to be a BIG day: 58 miles with 9,000 ft of climbing, including one 4,000 ft ascent and a 5,400 ft descent.

I woke at 6:30 to prep my bike, then had an OK corned beef hash for breakfast at 7:30. I also packed a wrap for lunch and a bag of mayonnaise and condiments that nobody ended up wanting — just extra weight! We left at 8:50am with the porch thermometer reading 30°F.

![Thermometer reading 30F]({{site.baseurl}}/assets/img/idaho/web/thermometer.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
30°F at Big Creek as we rolled out.
</span>

On the way out we stopped by the airstrip, since we'd seen a group fly in and out for dinner the night before. It's a pretty cool way to get around! The lodge even has a room named after a guy who was a naval aviator, a space shuttle pilot, and a Nevada backcountry pilot.

Then we immediately started the first climb of 3,000 ft, eventually making it to Elk Summit (8,670 ft) by 11:30am — the highest I've ever biked! I was definitely feeling the elevation, as I couldn't sustain a heart rate above 145 when I normally happily cruise at 160.

![Group at Elk Summit sign]({{site.baseurl}}/assets/img/idaho/web/elk-summit.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Elk Summit (8,670 ft), the highest I've ever biked.
</span>

Next came the long, rocky descent of more than a vertical mile. Unfortunately, about 1,000 ft into the descent I hit a rock with the edge of my rear wheel and blew out the sidewall of my tire. I had to patch it and put in a tube, and then we fought to inflate it without the pump pulling out the valve stem.

<video controls playsinline preload="metadata" poster="{{site.baseurl}}/assets/img/idaho/web/elk-descent-poster.jpg" style="display:block;margin:auto;max-width:100%;max-height:700px">
  <source src="{{site.baseurl}}/assets/img/idaho/web/elk-descent.mp4" type="video/mp4">
</video>
<span style="display: flex;justify-content: center;font-style:italic;">
Bouncing down the rocky descent from Elk Summit.
</span>

We eventually made it down the brutal descent after more than two hours of fighting rocks and gravel. I was glad for my shock-absorbing handlebars, but was still sore, especially my back.

We stopped at the Salmon River to quickly filter water from the stream, then immediately started the big, brutal 4,000 ft ascent. Everyone split up and climbed separately — some listening to music, some to podcasts, and some struggling in silence. We passed several remote homesteads and some friendly hunters with a whole herd of bird dogs, who cheered us on as we climbed. I was careful to keep my heart rate around 150 and drink plenty of Gatorade so I didn't bonk, but I was still frustrated and cranky and struggled at several points.

<video controls playsinline preload="metadata" poster="{{site.baseurl}}/assets/img/idaho/web/salmon-climb-poster.jpg" style="display:block;margin:auto;max-width:100%;max-height:700px">
  <source src="{{site.baseurl}}/assets/img/idaho/web/salmon-climb.mp4" type="video/mp4">
</video>
<span style="display: flex;justify-content: center;font-style:italic;">
Grinding up the 4,000 ft climb from the Salmon River.
</span>

<video controls playsinline preload="metadata" poster="{{site.baseurl}}/assets/img/idaho/web/salmon-climb-switchbacks-poster.jpg" style="display:block;margin:auto;max-width:100%;max-height:700px">
  <source src="{{site.baseurl}}/assets/img/idaho/web/salmon-climb-switchbacks.mp4" type="video/mp4">
</video>
<span style="display: flex;justify-content: center;font-style:italic;">
Switchbacks cut into the canyon wall.
</span>

![Riding through yellow aspens near Warren Summit]({{site.baseurl}}/assets/img/idaho/web/warren-summit-climb.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Yellow aspens near Warren Summit.
</span>

After reaching Warren Summit, we cruised downhill to Warren for lunch at the Baum Shelter. Once again the people were super friendly. There were lots of dogs, and the waitress showed us a communal fish pond full of rainbow trout. Warren has an official population of 8, but once had a population of 1,300 Chinese miners during the gold rush. There's no power or cell service — the town gets all its electricity from solar and its internet from Starlink.

We left at 6:35pm as it started raining, and made it over the last 900 ft ascent before putting on more warm clothes and lights for the descent in the dark and the final push to Burgdorf Hot Springs.

We ended up riding in a pace line with Dan pulling us along good gravel roads in the pitch dark and rain. It was actually quite fun! It was a good cardio pace, it was cool to see our lights make a single patch of bright light on the road, and there was a real sense of solidarity as we worked hard toward the destination together.

After 1.5 hours of riding in the dark and rain, we finally arrived at Burgdorf around 8:50pm. There was nobody at the desk, but Hazel found a back way into the lobby. We dropped our stuff in the cabin, started a fire, then headed down to the hot springs to soak.

The springs felt so good! There was a tub with a great cascade of warm water to wash off in, then a big pool of warm water with a lovely gravel bottom, and a super-hot tub under a tin roof. Soaking while listening to the patter of rain on the roof was divine! Even the changing room had a heated floor.

We stayed in "The Castle" cabin, which had two floors: a wood stove on the first, and five queen beds on the second, with a staircase hewn from a single log connecting them. Boris and Hazel had dropped off PB&J supplies, Pop-Tarts, chips, and sleeping bags earlier, so we slept warm and had a hearty breakfast in the morning. There was no electricity in the cabin, however, so I left my phone and watch charging in the corner of the lobby. I popped half a Unisom and was asleep by 11.

## Day 3: Loon Lake, Secesh River, and the Long Descent Home

I woke at 7:30, used the quite-nice privy, and grabbed hot coffee from the lobby.

![Steam rising from Burgdorf at dawn]({{site.baseurl}}/assets/img/idaho/web/burgdorf-morning.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Steam rising over Burgdorf Hot Springs at dawn.
</span>

![Steaming hot spring pool]({{site.baseurl}}/assets/img/idaho/web/burgdorf-pool.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
The main pool at Burgdorf, steaming in the cold morning air.
</span>

After prepping my gear and eating a Pop-Tart and PB&J, I moved my extra gear to the lobby. We were going to travel light and ride singletrack to Loon Lake and the crashed B-23 bomber.

Boris ate it on the icy front ramp — his first fall of the trip — and his bags went flying. Luckily he only bruised his left cheek and his dignity.

![Bikes parked by the woodshed at Burgdorf]({{site.baseurl}}/assets/img/idaho/web/burgdorf-woodshed.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Bikes stashed by the woodshed at Burgdorf.
</span>

We rolled out from Burgdorf at 9:50am toward Loon Lake. The mountain bike trail started out great, and we had a blast ripping downhill through the woods. Near the bottom the trail got more gravelly and steep and we had to walk a bit, but it was still fun.

![Riding through the pine forest]({{site.baseurl}}/assets/img/idaho/web/loon-lake-trail.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Riding through the pines toward Loon Lake.
</span>

We arrived at Loon Lake around noon and discovered a massive muddy puddle blocking our path. We managed to get our bikes across without getting too dirty, but quickly discovered more puddles blocking the way to the bomber. We decided to turn around, but the trail we'd come down was steep and technical, and would have been miserable to ride back up.

![Riding through a muddy, sandy puddle]({{site.baseurl}}/assets/img/idaho/web/loon-lake-puddle.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
Fording the puddles near Loon Lake.
</span>

Looking at the map, we saw a route on "motorized" trails along the Lick Creek trail to the Secesh River trail that would take us back to the gravel roads we'd ridden the night before. However, the trails were unknown, and the steep singletrack we'd taken into Loon Lake was also listed as "motorized," which seemed impossible. We decided to gamble and headed out along Lick Creek.

We immediately encountered huge rocks, broken bridges, and narrow gravel trails on sheer cliffs where we had to constantly carry or walk our bikes. Not a great start!

![Hike-a-bike on a rocky trail]({{site.baseurl}}/assets/img/idaho/web/lick-creek-hike-a-bike.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
Lots of hike-a-bike on the Lick Creek trail.
</span>

The trail was incredibly fun at points, but also quite harrowing. Near the intersection with the Secesh River trail, Boris took another spill, going over his bars and landing on his cheek again. Luckily it was only a few bruises and scratches!

We crossed a beautiful bridge over the Secesh River and started the trail out, which unfortunately required a ton more hike-a-bike and was slow going. It was a slog, and I gave away most of the food I'd brought, since we hadn't expected to be out so long and people were bonking. But after 2.5 miles of struggling, the trail flattened out into beautiful, flowy singletrack along the placid river for the last 1.5 miles. When we finally reached the campground trailhead, several of us collapsed in exhaustion — particularly Hazel.

![Singletrack along the Secesh River]({{site.baseurl}}/assets/img/idaho/web/secesh-singletrack.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Finally, flowy singletrack along the Secesh River.
</span>

From there it was a fairly fast ride on gravel roads to Yeti Backcountry Pizza, where we arrived around 2:30 for lunch. The owner was the nicest person we met on the whole trip, and the pizza and sandwiches were pretty good. I'm glad we got to try it!

![Group riding past a meadow]({{site.baseurl}}/assets/img/idaho/web/secesh-meadow.jpg)
<span style="display: flex;justify-content: center;font-style:italic;">
Back on gravel and headed for pizza.
</span>

After lunch we headed back out and rode to Burgdorf on the same roads we'd done the night before. It was great to see them in daylight, and fun to ride in a pace line again! But my legs were dead after three big days of riding, and I was definitely bringing up the rear.

![Gravel road through tall pines]({{site.baseurl}}/assets/img/idaho/web/burgdorf-road.jpg){: style="display:block;margin:auto;max-height:700px"}
<span style="display: flex;justify-content: center;font-style:italic;">
The road back to Burgdorf, finally in daylight.
</span>

At Burgdorf we loaded our bikes with all the extra gear Boris had dropped off. I stuffed two sleeping bags into my panniers and strapped a big pack full of other gear to the top of my rack. It made for a big rear end, but it was nice not to carry anything on my back and to keep the weight low. Rylan carried an entire hiking pack on her back!

After a short ride from Burgdorf to the main road, we hit asphalt and started the last climb of the trip: 400 ft up to Secesh Summit. From there we began the 25-mile descent back to McCall and had a BLAST!

<video controls playsinline preload="metadata" poster="{{site.baseurl}}/assets/img/idaho/web/secesh-descent-poster.jpg" style="display:block;margin:auto;max-width:100%;max-height:700px">
  <source src="{{site.baseurl}}/assets/img/idaho/web/secesh-descent.mp4" type="video/mp4">
</video>
<span style="display: flex;justify-content: center;font-style:italic;">
Flying down the pavement from Secesh Summit.
</span>

We hit 40mph despite my chunky 45mm tires and my massive behind, passing beautiful sunny vistas, lakes, and yellow quaking aspens. We finally rolled back into McCall and our car at 6:10pm. The only negative was a jerk in a truck blasting a train horn at us and nearly blowing out our eardrums as we rolled into town.

After unpacking and loading the cars (with a few discreet quick changes out of our sweaty clothes), we headed to dinner at North 55 Social in Cascade. We had a hearty dinner to the dulcet background tones of "Let the Bodies Hit the Floor" and shared our highs and lows from the trip.

After dinner we drove to our Airbnb in Boise. They hadn't given us the door code, so I had to break in through the back door. I had a quick dip in the hot tub before passing out in the king bed with Jordan by 11.

## Day 4: The Drive Home

I woke at 6:30am to load the car and said goodbye to the people flying home. Boris and I were on the road by 8, with breakfast at Primal Coffee, a local hipster coffee shop, before the long drive. We almost ran out of gas, putting 16.1 gallons into a 16-gallon tank in Jordan Valley!

We stopped for lunch at the Good Diggers Saloon and Grubhouse, which felt like a Burning Man camp, then drove straight back to SF with only short stops for gas, snacks, and bathroom breaks.

## Gear and Logistics

A few notes if you want to attempt this yourself! We did the route on gravel bikes, but hard-tail MTB might be nicer for the Day 2 descents and the trails to Loon Lake. I rode a Cannondale Topstone 2 Alloy with 45mm Schwalbe G-One RX Pro tires and a RockShox suspension stem, which was a godsend on the rocky descents. For bags, I had a top tube bag and two panniers (Arkel Dry-Lite bags on a Tailfin rack), which was more than enough space for all my gear since we slept indoors both nights. Most of the others ran 45mm tires and carried their gear in under-seat bags, but a few had 50mm or 55mm tires, which were even smoother on the descents.

Big Creek Lodge and Burgdorf Hot Springs both require reservations, and it's easiest to call. Also, note that Burgdorf cabins have no power, but the lobby does have electricity and they're super friendly to let you charge items. You also have to pack in and out all your food and trash from the hot springs. There's apparently also lodging near The Corner and near Yeti Backcountry Pizza, but you might have to call and ask for details on those places. There was no service on the majority of the route, but pretty much every place we stopped had free Wifi through starlink, which allowed us to check in with friends and family frequently each day. 
