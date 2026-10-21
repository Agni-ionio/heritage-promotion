---
layout: page
title: "🗺️ Προορισμοί"
permalink: /pois/
---

<style>
.poi-grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
  gap:20px;
  margin-bottom:40px;
}

.poi-card{
  border:1px solid #ddd;
  border-radius:12px;
  padding:20px;
  background:white;
  box-shadow:0 2px 8px rgba(0,0,0,0.1);
  transition:all 0.3s ease;
}

.poi-card:hover{
  transform:translateY(-4px);
  box-shadow:0 6px 16px rgba(0,0,0,0.15);
}

.poi-card a{
  text-decoration:none;
  font-weight:bold;
  color:#2c3e50;
  font-size:1.05rem;
}

.poi-card a:hover{
  color:#0077b6;
}

.category{
  margin-top:50px;
}

.category h2{
  margin-bottom:10px;
}

.category p{
  color:#555;
  margin-bottom:20px;
  line-height:1.6;
}

.intro{
  font-size:1.1rem;
  line-height:1.8;
  margin-bottom:40px;
  text-align:center;
  color:#444;
}
</style>

<div class="intro">
  Ανακαλύψτε επιλεγμένους φυσικούς προορισμούς της Ελλάδας,
  οργανωμένους ανά κατηγορία τοπίου και εμπειρίας.
  Κάθε προορισμός προσφέρει μοναδικές εικόνες, δραστηριότητες
  και ευκαιρίες εξερεύνησης της Ελληνικής φύσης.
</div>

<div class="category">
  <h2>🌿 Λίμνες &amp; Γαλήνια Φύση</h2>
  <p>Ήρεμα τοπία, φυσική ομορφιά και εμπειρίες χαλάρωσης δίπλα στο νερό.</p>

  <div class="poi-grid">
    {% for p in site.pois %}
      {% if p.category == "lake" %}
        <div class="poi-card">
          <a href="{{ p.url | relative_url }}">
            {{ p.title }}
          </a>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>

<div class="category">
  <h2>🌊 Ποτάμια &amp; Δραστηριότητες</h2>
  <p>
    Κρυστάλλινα νερά, φαράγγια και μοναδικές εμπειρίες περιπέτειας
    στην καρδιά της φύσης.
  </p>

  <div class="poi-grid">
    {% for p in site.pois %}
      {% if p.category == "river" %}
        <div class="poi-card">
          <a href="{{ p.url | relative_url }}">
            {{ p.title }}
          </a>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>

<div class="category">
  <h2>🏔️ Ορεινές Αποδράσεις &amp; Περιπέτεια</h2>
  <p>
    Αλπικά τοπία, εντυπωσιακά φαράγγια και πεζοπορικές διαδρομές
    για όσους αγαπούν την εξερεύνηση.
  </p>

  <div class="poi-grid">
    {% for p in site.pois %}
      {% if p.category == "mountain" %}
        <div class="poi-card">
          <a href="{{ p.url | relative_url }}">
            {{ p.title }}
          </a>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>
