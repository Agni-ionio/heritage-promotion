---
layout: page
title: "Χάρτης Προορισμών"
permalink: /map/
---

<div id="all-pois-map" style="height: 700px; width: 100%; border-radius: 12px; margin-top: 20px;"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.8.0/dist/leaflet.css" />
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css" />
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css" />

<script src="https://unpkg.com/leaflet@1.8.0/dist/leaflet.js"></script>
<script src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"></script>

<script>
  const map = L.map('all-pois-map').setView([39.1, 21.4], 7);

  L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '&copy; OpenStreetMap contributors'
  }).addTo(map);

  const markers = L.markerClusterGroup({
    showCoverageOnHover: false,
    maxClusterRadius: 50
  });

  const pois = [
    {% for p in site.pois %}
      {% if p.wikidatum %}
      {
        title: "{{ p.title | escape }}",
        url: "{{ p.url | relative_url }}",
        wikidatum: "{{ p.wikidatum }}"
      },
      {% endif %}
    {% endfor %}
  ];

  function getJsonValue(path, obj) {
    try {
      let current = obj;
      for (const key of path) {
        current = current[key];
      }
      return current;
    } catch (e) {
      return null;
    }
  }

  function getCommonsImageUrl(fileName) {
    if (!fileName) return '';
    const safe = fileName.replace(/ /g, '_');
    return `https://commons.wikimedia.org/wiki/Special:FilePath/${encodeURIComponent(safe)}?width=220`;
  }

  async function fetchPoiData(poi) {
    const url = `https://www.wikidata.org/w/api.php?action=wbgetentities&ids=${poi.wikidatum}&format=json&languages=en|el&origin=*`;
    const response = await fetch(url);
    const data = await response.json();

    const coords = getJsonValue(
      ['entities', poi.wikidatum, 'claims', 'P625', 0, 'mainsnak', 'datavalue', 'value'],
      data
    );

    if (!coords) return null;

    const title = getJsonValue(
      ['entities', poi.wikidatum, 'sitelinks', 'elwiki', 'title'],
      data
    ) || poi.title;

    const imageName = getJsonValue(
      ['entities', poi.wikidatum, 'claims', 'P18', 0, 'mainsnak', 'datavalue', 'value'],
      data
    );

    return {
      ...poi,
      title,
      lat: coords.latitude,
      lng: coords.longitude,
      imageUrl: getCommonsImageUrl(imageName)
    };
  }

  async function loadAllPois() {
    const results = await Promise.all(
      pois.map(fetchPoiData)
    );

    results.filter(Boolean).forEach((poi) => {
      const marker = L.marker([poi.lat, poi.lng]);

      const popupHtml =
        '<div style="min-width: 220px; max-width: 240px;">' +
        (poi.imageUrl ? '<img src="' + poi.imageUrl + '" style="width: 100%; height: 120px; object-fit: cover; border-radius: 8px; margin-bottom: 8px;" />' : '') +
        '<div style="font-weight: 700; margin-bottom: 6px;">' + poi.title + '</div>' +
        '<a href="' + poi.url + '" target="_blank" rel="noopener">Προορισμός</a>' +
        '</div>';

      marker.bindPopup(popupHtml);
      markers.addLayer(marker);
    });

    map.addLayer(markers);
  }

  loadAllPois();
</script>
