---
layout: post

title: "Game of Robes"
image: /assets/images/posts/2025/05/2025-05-05-game-of-robes/featured.png
date: 2025-05-05
category: Religion
tags: [catholic, pope, conclave]
---

<!-- Load D3 first -->
<script src="https://d3js.org/d3.v7.min.js"></script>
<script src="https://d3js.org/d3.v7.min.js"></script>
<script src="https://d3js.org/topojson.v3.min.js"></script>

<style>
a {
  color: #C41E3A !important;
}
</style>

When a pope dies or resigns, 120 cardinals gather in the Sistine Chapel, lock the doors, and hold one of the most secretive elections in the world. No speeches. No campaigns. Just a series of silent ballots, and eventually, white smoke.

The [papal conclave](https://en.wikipedia.org/wiki/Papal_conclave) has selected new popes for nearly 800 years. The word itself means "with key", a reference to the literal locking-in of the cardinals. The rules have evolved since the 13th century, but the process remains shrouded in mystery. What we do know comes from leaks, betting markets, and rare historical diaries.

In 2025, the cardinals may once again gather to choose a new pope. But what factors determine the outcome? Geography? Politics? Theology? Divine Intervention?

We've explored 800 years of conclave history to try and find the answer. Let's explore how much can be known before the smoke rises!

## The College of Cardinals

<img src="/assets/images/posts/2025/05/2025-05-05-game-of-robes/section_1.png" alt="The College of Cardinals" loading="lazy" width="100%" height="auto" />

Only cardinals under the age of 80 can vote in a papal conclave. As of 2025, there are 135 eligible electors. Nearly four out of five were appointed by Pope Francis.

<div>
<div id="ordaining-pope-chart" class="w-100" style="max-width: 700px; margin: 3rem auto;"></div>

<style>
  .pope-label {
    font-family: sans-serif;
    font-size: 14px;
    fill: #333;
  }
  .pope-bar text {
    font-size: 12px;
    fill: #fff;
    text-anchor: end;
    alignment-baseline: middle;
  }
</style>

{% raw %}
<script>
window.addEventListener("DOMContentLoaded", () => {
  const popeData = [
    {
      pope: "Francis",
      count: 108,
      image: "https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Pope_Francis_Korea_Haemi_Castle_19.jpg/500px-Pope_Francis_Korea_Haemi_Castle_19.jpg"
    },
    {
      pope: "Benedict XVI",
      count: 22,
      image: "https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/Benedykt_XVI_%282010-10-17%29_2.jpg/500px-Benedykt_XVI_%282010-10-17%29_2.jpg"
    },
    {
      pope: "John Paul II",
      count: 5,
      image: "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/ADAMELLO_-_PAPA_-_Giovanni_Paolo_II_-_panoramio_%28cropped%29.jpg/500px-ADAMELLO_-_PAPA_-_Giovanni_Paolo_II_-_panoramio_%28cropped%29.jpg"
    }
  ];

  const margin = { top: 20, right: 20, bottom: 20, left: 130 },
        width = 700 - margin.left - margin.right,
        height = popeData.length * 80;

  const svg = d3.select("#ordaining-pope-chart")
    .append("svg")
    .attr("viewBox", `0 0 ${width + margin.left + margin.right} ${height + margin.top + margin.bottom}`)
    .attr("preserveAspectRatio", "xMidYMid meet")
    .append("g")
    .attr("transform", `translate(${margin.left},${margin.top})`);

  const x = d3.scaleLinear()
    .domain([0, d3.max(popeData, d => d.count)])
    .range([0, width]);

  const y = d3.scaleBand()
    .domain(popeData.map(d => d.pope))
    .range([0, height])
    .padding(0.3);

  // Bars
  svg.append("g")
    .selectAll("rect")
    .data(popeData)
    .join("rect")
    .attr("x", 80)
    .attr("y", d => y(d.pope))
    .attr("width", d => x(d.count))
    .attr("height", y.bandwidth())
    .attr("fill", "#C41E3A");

  // Bar count labels at START of bar
    svg.append("g")
    .selectAll("text.count")
    .data(popeData)
    .join("text")
    .attr("x", d => {
        const barStart = 80;
        const barWidth = x(d.count);
        return barWidth > 30 ? barStart + 5 : barStart + barWidth + 5;
    })
    .attr("y", d => y(d.pope) + y.bandwidth() / 2)
    .text(d => d.count)
    .attr("class", "pope-bar text")
    .attr("text-anchor", d => x(d.count) > 30 ? "start" : "start")
    .attr("alignment-baseline", "middle")
    .style("fill", d => x(d.count) > 30 ? "#fff" : "#000");


  // Pope portraits
  svg.append("g")
    .selectAll("image")
    .data(popeData)
    .join("image")
    .attr("xlink:href", d => d.image)
    .attr("x", -70)
    .attr("y", d => y(d.pope))
    .attr("width", 60)
    .attr("height", y.bandwidth())
    .attr("preserveAspectRatio", "xMidYMid slice");

  // Pope name labels
  svg.append("g")
    .selectAll("text.label")
    .data(popeData)
    .join("text")
    .text(d => d.pope)
    .attr("x", -10)
    .attr("y", d => y(d.pope) + y.bandwidth() / 2)
    .attr("class", "pope-label")
    .attr("text-anchor", "end")
    .attr("alignment-baseline", "middle");
});
</script>
{% endraw %}
</div>


Pope Francis’s legacy is visible not only in his writings and reforms but also in the very body that will one day elect his successor. Having named more cardinals than John Paul II or Benedict XVI, he has created a remarkably large cohort of his appointees. Were they ever to vote together, their numbers alone could tip the balance. 

The chart below shows how the current college of electors breaks down by the pope who ordained them.


<div>
<div class="container my-5">
  <div class="row justify-content-center text-center g-4">

    
      <div class="col-12 col-md-4">
        <h4 class="fw-bold mb-2">John Paul II</h4>

        <div class="d-flex flex-wrap justify-content-center gap-1">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Philippe_Barbarin_%28cropped%29.jpg/250px-Philippe_Barbarin_%28cropped%29.jpg"
                 alt="Philippe Barbarin"
                 title="Philippe Barbarin"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/61/CardinalJosipBozani%C4%87.jpg/250px-CardinalJosipBozani%C4%87.jpg"
                 alt="Josip Bozanić"
                 title="Josip Bozanić"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/P%C3%A9ter_Erd%C5%91_in_2011.jpg/250px-P%C3%A9ter_Erd%C5%91_in_2011.jpg"
                 alt="Péter Erdő"
                 title="Péter Erdő"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Vinko_Puljic_June_2015_%2818760757238%29.jpg/250px-Vinko_Puljic_June_2015_%2818760757238%29.jpg"
                 alt="Vinko Puljić"
                 title="Vinko Puljić"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/32/Peter_Turkson_%28cropped%29.jpg/250px-Peter_Turkson_%28cropped%29.jpg"
                 alt="Peter Kodwo Appiah Turkson"
                 title="Peter Kodwo Appiah Turkson"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
        </div>
      </div>
    
      <div class="col-12 col-md-4">
        <h4 class="fw-bold mb-2">Benedict XVI</h4>

        <div class="d-flex flex-wrap justify-content-center gap-1">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7f/Joao_braz_de_aviz.jpg/250px-Joao_braz_de_aviz.jpg"
                 alt="João Bráz de Aviz"
                 title="João Bráz de Aviz"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/80/Betori.JPG/250px-Betori.JPG"
                 alt="Giuseppe Betori"
                 title="Giuseppe Betori"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/32/Burke-BB-768x1152.jpg/250px-Burke-BB-768x1152.jpg"
                 alt="Raymond Leo Burke"
                 title="Raymond Leo Burke"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Card._Canizares_%2830279529724%29.jpg/250px-Card._Canizares_%2830279529724%29.jpg"
                 alt="Antonio Cañizares Llovera"
                 title="Antonio Cañizares Llovera"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/51/Thomas_Christopher_Collins_2014.jpg/250px-Thomas_Christopher_Collins_2014.jpg"
                 alt="Thomas Christopher Collins"
                 title="Thomas Christopher Collins"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Daniel_DiNardo_%28142360355%29.jpg/250px-Daniel_DiNardo_%28142360355%29.jpg"
                 alt="Daniel Nicholas DiNardo"
                 title="Daniel Nicholas DiNardo"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/Willem_Eijk_%282020%29.jpg/250px-Willem_Eijk_%282020%29.jpg"
                 alt="Willem Jacobus Eijk"
                 title="Willem Jacobus Eijk"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Card._Fernando_Filoni_in_Taiwan.jpg/250px-Card._Fernando_Filoni_in_Taiwan.jpg"
                 alt="Fernando Filoni"
                 title="Fernando Filoni"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cd/James_Cardinal_Harvey_%28cropped%29.JPG/250px-James_Cardinal_Harvey_%28cropped%29.JPG"
                 alt="James Michael Harvey"
                 title="James Michael Harvey"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Kurt_Koch_par_Claude_Truong-Ngoc_d%C3%A9cembre_2016.jpg/250px-Kurt_Koch_par_Claude_Truong-Ngoc_d%C3%A9cembre_2016.jpg"
                 alt="Kurt Koch"
                 title="Kurt Koch"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Coat_of_arms_of_Reinhard_Marx.svg/250px-Coat_of_arms_of_Reinhard_Marx.svg.png"
                 alt="Reinhard Marx"
                 title="Reinhard Marx"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2c/Cardinal_John_Njue_in_February_2017_in_the_Vatican_City.jpg/250px-Cardinal_John_Njue_in_February_2017_in_the_Vatican_City.jpg"
                 alt="John Njue"
                 title="John Njue"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ec/K._nycz1_%28cropped%29.jpg/250px-K._nycz1_%28cropped%29.jpg"
                 alt="Kazimierz Nycz"
                 title="Kazimierz Nycz"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Cardinal_Ranjith.jpg/250px-Cardinal_Ranjith.jpg"
                 alt="Albert Malcolm Ranjith Patabendige Don"
                 title="Albert Malcolm Ranjith Patabendige Don"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/07/Stanis%C5%82aw_Ry%C5%82ko_%282018%29.jpg/250px-Stanis%C5%82aw_Ry%C5%82ko_%282018%29.jpg"
                 alt="Stanisław Ryłko"
                 title="Stanisław Ryłko"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/Cardinal_Robert_Sarah_%28cropped%29.JPG/250px-Cardinal_Robert_Sarah_%28cropped%29.JPG"
                 alt="Robert Sarah"
                 title="Robert Sarah"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Odilo_Scherer_em_setembro_de_2018_%28cropped%29.jpg/250px-Odilo_Scherer_em_setembro_de_2018_%28cropped%29.jpg"
                 alt="Odilo Pedro Scherer"
                 title="Odilo Pedro Scherer"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/60/Luis_Antonio_Tagle_at_Manila_in_2016_%28cropped%29.jpg/250px-Luis_Antonio_Tagle_at_Manila_in_2016_%28cropped%29.jpg"
                 alt="Luis Antonio Gokim Tagle"
                 title="Luis Antonio Gokim Tagle"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/67/Cardinal_Baselios_Cleemis_Cardinal_Thottunkal.jpg/250px-Cardinal_Baselios_Cleemis_Cardinal_Thottunkal.jpg"
                 alt="Baselios Cleemis (Isaac) Thottunkal"
                 title="Baselios Cleemis (Isaac) Thottunkal"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
        </div>
      </div>
    
      <div class="col-12 col-md-4">
        <h4 class="fw-bold mb-2">Francis</h4>

        <div class="d-flex flex-wrap justify-content-center gap-1">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/aa/Portrait_of_Cardinal_Advincula.jpg/250px-Portrait_of_Cardinal_Advincula.jpg"
                 alt="Jose Lazaro Fuerte Advincula Jr."
                 title="Jose Lazaro Fuerte Advincula Jr."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/74/Cardenalaguiar.jpg/250px-Cardenalaguiar.jpg"
                 alt="Carlos Aguiar Retes"
                 title="Carlos Aguiar Retes"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/60/Mons._Americo_Aguiar_visita_a_los_j%C3%B3venes_de_Tierra_Santa%2C_2023.png/250px-Mons._Americo_Aguiar_visita_a_los_j%C3%B3venes_de_Tierra_Santa%2C_2023.png"
                 alt="Américo Manuel Alves Aguiar"
                 title="Américo Manuel Alves Aguiar"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/AED_11%C3%A8me_Nuit_des_T%C3%A9moins_-_Fridolin_Ambongo_-_2_%28cropped%29.jpg/250px-AED_11%C3%A8me_Nuit_des_T%C3%A9moins_-_Fridolin_Ambongo_-_2_%28cropped%29.jpg"
                 alt="Fridolin Ambongo Besungu, O.F.M. Cap."
                 title="Fridolin Ambongo Besungu, O.F.M. Cap."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Anders_Arborelius_in_2019.jpg/250px-Anders_Arborelius_in_2019.jpg"
                 alt="Anders Arborelius, O.C.D."
                 title="Anders Arborelius, O.C.D."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/Mgr_Jean-Marc_Aveline_par_Claude_Truong-Ngoc_juillet_2021.jpg/250px-Mgr_Jean-Marc_Aveline_par_Claude_Truong-Ngoc_juillet_2021.jpg"
                 alt="Jean-Marc Noël Aveline"
                 title="Jean-Marc Noël Aveline"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Fabio_Baggio-5-2-2019.jpg/250px-Fabio_Baggio-5-2-2019.jpg"
                 alt="Fabio Baggio, C.S."
                 title="Fabio Baggio, C.S."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8b/Don_Mimmo_Battaglia_pentecoste_2018_%28cropped%29.jpg/250px-Don_Mimmo_Battaglia_pentecoste_2018_%28cropped%29.jpg"
                 alt="Domenico Battaglia"
                 title="Domenico Battaglia"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/16/Ignace_Bessi_Dogbo.png/250px-Ignace_Bessi_Dogbo.png"
                 alt="Ignace Bessi Dogbo"
                 title="Ignace Bessi Dogbo"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cd/Maung_Bo.jpg/250px-Maung_Bo.jpg"
                 alt="Charles Maung Bo, S.D.B."
                 title="Charles Maung Bo, S.D.B."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/96/Stephen_Brislin.jpg/250px-Stephen_Brislin.jpg"
                 alt="Stephen Brislin"
                 title="Stephen Brislin"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/Fran%C3%A7ois-Xavier_Bustillo_%28cropped%29.jpg/200px-Fran%C3%A7ois-Xavier_Bustillo_%28cropped%29.jpg"
                 alt="François-Xavier Bustillo, O.F.M. Conv."
                 title="François-Xavier Bustillo, O.F.M. Conv."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b0/Mykola_Bychok%2C_CSsR.jpg/250px-Mykola_Bychok%2C_CSsR.jpg"
                 alt="Mykola Bychok, C.Ss.R."
                 title="Mykola Bychok, C.Ss.R."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/58/Coat_of_arms_of_Luis_Gerardo_Cabrera_Herrera.svg/250px-Coat_of_arms_of_Luis_Gerardo_Cabrera_Herrera.svg.png"
                 alt="Luis Gerardo Cabrera Herrera, O.F.M."
                 title="Luis Gerardo Cabrera Herrera, O.F.M."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/19/Card._Cantoni_27-08-22.jpg/250px-Card._Cantoni_27-08-22.jpg"
                 alt="Oscar Cantoni"
                 title="Oscar Cantoni"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Coat_of_arms_of_Carlos_Gustavo_Castillo_Mattasoglio_%28cardinal%29.svg/250px-Coat_of_arms_of_Carlos_Gustavo_Castillo_Mattasoglio_%28cardinal%29.svg.png"
                 alt="Carlos Gustavo Castillo Mattasoglio"
                 title="Carlos Gustavo Castillo Mattasoglio"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/51/Arzobispo_Fernando_Chomali_%28cropped%29.jpg/250px-Arzobispo_Fernando_Chomali_%28cropped%29.jpg"
                 alt="Fernando Natalio Chomalí Garib"
                 title="Fernando Natalio Chomalí Garib"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c4/202309_CardinalStephenCHOW_1H_ForMedia.jpg/250px-202309_CardinalStephenCHOW_1H_ForMedia.jpg"
                 alt="Stephen Chow Sau-yan, S.J."
                 title="Stephen Chow Sau-yan, S.J."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/de/Manuel_Clemente.png/250px-Manuel_Clemente.png"
                 alt="Manuel José Macário do Nascimento Clemente"
                 title="Manuel José Macário do Nascimento Clemente"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/10/Toma_posesi%C3%B3n_de_Jos%C3%A9_Cobo_de_Santa_Mar%C3%ADa_de_Montserrat%2C_Roma_04.jpg/250px-Toma_posesi%C3%B3n_de_Jos%C3%A9_Cobo_de_Santa_Mar%C3%ADa_de_Montserrat%2C_Roma_04.jpg"
                 alt="José Cobo Cano"
                 title="José Cobo Cano"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/30/Cardeal_Costa.jpg/250px-Cardeal_Costa.jpg"
                 alt="Paulo Cezar Costa"
                 title="Paulo Cezar Costa"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/79/Mgr_Coutts_%C3%A0_la_Nuit_des_T%C3%A9moins_2016_%28cropped%29.jpg/250px-Mgr_Coutts_%C3%A0_la_Nuit_des_T%C3%A9moins_2016_%28cropped%29.jpg"
                 alt="Joseph Coutts"
                 title="Joseph Coutts"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f2/Cdl._Cupich_%28cropped%29.png/250px-Cdl._Cupich_%28cropped%29.png"
                 alt="Blase Joseph Cupich"
                 title="Blase Joseph Cupich"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b2/Card._Michael_Czerny_S.J._at_the_Vatican.jpg/250px-Card._Michael_Czerny_S.J._at_the_Vatican.jpg"
                 alt="Michael F. Czerny, S.J."
                 title="Michael F. Czerny, S.J."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/69/Pablo_Virgilio_David_in_2014_%28Retouched%29.jpg/250px-Pablo_Virgilio_David_in_2014_%28Retouched%29.jpg"
                 alt="Pablo Virgilio Siongco David"
                 title="Pablo Virgilio Siongco David"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Coat_of_arms_of_Angelo_De_Donatis_%28Cardinal%29.svg/250px-Coat_of_arms_of_Angelo_De_Donatis_%28Cardinal%29.svg.png"
                 alt="Angelo De Donatis"
                 title="Angelo De Donatis"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/Monseigneur_De_Kesel_%C3%A0_la_basilique_Saint-Martin_de_Li%C3%A8ge.jpg/250px-Monseigneur_De_Kesel_%C3%A0_la_basilique_Saint-Martin_de_Li%C3%A8ge.jpg"
                 alt="Jozef De Kesel"
                 title="Jozef De Kesel"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/John_Atcherley_Dew_in_2014.jpg/250px-John_Atcherley_Dew_in_2014.jpg"
                 alt="John Atcherley Dew"
                 title="John Atcherley Dew"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/2017-03-30_Virg%C3%ADlio_do_Carmo_da_Silva.jpg/250px-2017-03-30_Virg%C3%ADlio_do_Carmo_da_Silva.jpg"
                 alt="Virgilio do Carmo da Silva, S.D.B."
                 title="Virgilio do Carmo da Silva, S.D.B."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5e/Kevin_Joseph_Farrell_1.jpg/250px-Kevin_Joseph_Farrell_1.jpg"
                 alt="Kevin Joseph Farrell"
                 title="Kevin Joseph Farrell"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/24/V%C3%ADctor_Manuel_Fern%C3%A1ndez-%28cropped%29.jpg/250px-V%C3%ADctor_Manuel_Fern%C3%A1ndez-%28cropped%29.jpg"
                 alt="Victor Manuel Fernández"
                 title="Victor Manuel Fernández"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/%C3%81ngel_Fern%C3%A1ndez_Artime.jpg/250px-%C3%81ngel_Fern%C3%A1ndez_Artime.jpg"
                 alt="Ángel Fernández Artime, S.D.B."
                 title="Ángel Fernández Artime, S.D.B."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a5/Filipe_Neri_Ferrao_%28cropped%29.jpg/250px-Filipe_Neri_Ferrao_%28cropped%29.jpg"
                 alt="Filipe Neri António Sebastião do Rosário Ferrão"
                 title="Filipe Neri António Sebastião do Rosário Ferrão"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Sebastian_Francis.jpg/250px-Sebastian_Francis.jpg"
                 alt="Sebastian Francis"
                 title="Sebastian Francis"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b2/Arlindo_Gomes_Furtado.jpg/250px-Arlindo_Gomes_Furtado.jpg"
                 alt="Arlindo Gomes Furtado"
                 title="Arlindo Gomes Furtado"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1a/Cardinale_Mauro_Gambetti.png/250px-Cardinale_Mauro_Gambetti.png"
                 alt="Mauro Maria Gambetti, O.F.M. Conv."
                 title="Mauro Maria Gambetti, O.F.M. Conv."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/Cardinal_William_Goh_%28cropped%29.jpg/250px-Cardinal_William_Goh_%28cropped%29.jpg"
                 alt="William Goh Seng Chye"
                 title="William Goh Seng Chye"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Cardinal_Mario_Grech_%28cropped%29.jpg/250px-Cardinal_Mario_Grech_%28cropped%29.jpg"
                 alt="Mario Grech"
                 title="Mario Grech"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b2/Claudio_Gugerotti_1.jpg/250px-Claudio_Gugerotti_1.jpg"
                 alt="Claudio Gugerotti"
                 title="Claudio Gugerotti"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1c/Jean-Claude_Hollerich.jpg/250px-Jean-Claude_Hollerich.jpg"
                 alt="Jean-Claude Hollerich, S.J."
                 title="Jean-Claude Hollerich, S.J."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/33/His_Eminence_Antoine_cardinal_Kambanda%2C_GCLJ.jpg/250px-His_Eminence_Antoine_cardinal_Kambanda%2C_GCLJ.jpg"
                 alt="Antoine Kambanda"
                 title="Antoine Kambanda"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/84/Cardinal_Tarsicio_Isao_Kikuchi.jpg/250px-Cardinal_Tarsicio_Isao_Kikuchi.jpg"
                 alt="Tarcisio Isao Kikuchi, S.V.D."
                 title="Tarcisio Isao Kikuchi, S.V.D."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bd/Card._George_J._Koovakad.jpg/250px-Card._George_J._Koovakad.jpg"
                 alt="George Jacob Koovakad"
                 title="George Jacob Koovakad"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Konrad_Krajewski_%282020%29.jpg/250px-Konrad_Krajewski_%282020%29.jpg"
                 alt="Konrad Krajewski"
                 title="Konrad Krajewski"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/Jean_Pierre_Kutwa.jpg/250px-Jean_Pierre_Kutwa.jpg"
                 alt="Jean-Pierre Kutwa"
                 title="Jean-Pierre Kutwa"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1c/Mouvaux_-_Cardinal_G%C3%A9rald_Cyprien_Lacroix_-_1_%28cropped%29.jpg/250px-Mouvaux_-_Cardinal_G%C3%A9rald_Cyprien_Lacroix_-_1_%28cropped%29.jpg"
                 alt="Gérald Cyprien Lacroix, I.S.P.X."
                 title="Gérald Cyprien Lacroix, I.S.P.X."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0a/President_Ma_and_Chibly_Langlois_%2817262956123%29_%28cropped%29.jpg/250px-President_Ma_and_Chibly_Langlois_%2817262956123%29_%28cropped%29.jpg"
                 alt="Chibly Langlois"
                 title="Chibly Langlois"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/de/Augusto_Paolo_Lojudice_con_il_pallio.jpg/250px-Augusto_Paolo_Lojudice_con_il_pallio.jpg"
                 alt="Augusto Paolo Lojudice"
                 title="Augusto Paolo Lojudice"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/55/Crist%C3%B3bal_L%C3%B3pez_Romero.jpg/250px-Crist%C3%B3bal_L%C3%B3pez_Romero.jpg"
                 alt="Cristóbal López Romero, S.D.B."
                 title="Cristóbal López Romero, S.D.B."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Archbishop_Thomas_Aquinas_Manyo_Maeda_in_2015.jpg/200px-Archbishop_Thomas_Aquinas_Manyo_Maeda_in_2015.jpg"
                 alt="Thomas Aquino Manyo Maeda"
                 title="Thomas Aquino Manyo Maeda"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/66/Soane_Patita_Paini_Mafi.jpg/250px-Soane_Patita_Paini_Mafi.jpg"
                 alt="Soane Patita Paini Mafi"
                 title="Soane Patita Paini Mafi"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/ba/Rolandas_makrickas-1_%28edit%29.jpg/250px-Rolandas_makrickas-1_%28edit%29.jpg"
                 alt="Rolandas Makrickas"
                 title="Rolandas Makrickas"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f2/Dominique_Mamberti%2C_14_Dec_2010.jpg/250px-Dominique_Mamberti%2C_14_Dec_2010.jpg"
                 alt="Dominique François Joseph Mamberti"
                 title="Dominique François Joseph Mamberti"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/10/Cardeal_marengo.jpg/250px-Cardeal_marengo.jpg"
                 alt="Giorgio Marengo, I.M.C."
                 title="Giorgio Marengo, I.M.C."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Adalberto_Mart%C3%ADnez_Flores.jpg/250px-Adalberto_Mart%C3%ADnez_Flores.jpg"
                 alt="Adalberto Martínez Flores"
                 title="Adalberto Martínez Flores"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/74/Francesco_Montenegro_concistoro.jpg/250px-Francesco_Montenegro_concistoro.jpg"
                 alt="Francesco Montenegro"
                 title="Francesco Montenegro"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/df/Ameyu_Martin_Mulla_%28edit%29.jpg/250px-Ameyu_Martin_Mulla_%28edit%29.jpg"
                 alt="Stephen Ameyu Martin Mulla"
                 title="Stephen Ameyu Martin Mulla"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1d/Bischof-GL-M%C3%BCller.JPG/250px-Bischof-GL-M%C3%BCller.JPG"
                 alt="Gerhard Ludwig Müller"
                 title="Gerhard Ludwig Müller"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/Coat_of_arms_of_Ladislav_Nemet_%28cardinal%29.svg/250px-Coat_of_arms_of_Ladislav_Nemet_%28cardinal%29.svg.png"
                 alt="Ladislav Nemet, S.V.D."
                 title="Ladislav Nemet, S.V.D."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/15/Archbishop_Vincent_Nichols_%287999797353%29_%28cropped%29.jpg/250px-Archbishop_Vincent_Nichols_%287999797353%29_%28cropped%29.jpg"
                 alt="Vincent Gerard Nichols"
                 title="Vincent Gerard Nichols"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Dieudonn%C3%A9_Nzapala%C3%AFnga_VOA_cropped.jpg/250px-Dieudonn%C3%A9_Nzapala%C3%AFnga_VOA_cropped.jpg"
                 alt="Dieudonné Nzapalainga, C.S.Sp."
                 title="Dieudonné Nzapalainga, C.S.Sp."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/ba/Mons._Osoro_%2830877902606%29.jpg/250px-Mons._Osoro_%2830877902606%29.jpg"
                 alt="Carlos Osoro Sierra"
                 title="Carlos Osoro Sierra"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/00/Cardinal_Pietro_Parolin_par_Claude_Truong-Ngoc_juillet_2021.jpg/250px-Cardinal_Pietro_Parolin_par_Claude_Truong-Ngoc_juillet_2021.jpg"
                 alt="Pietro Parolin"
                 title="Pietro Parolin"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/13/Foto_Cardinale_Petrocchi_-_cropped.jpg/250px-Foto_Cardinale_Petrocchi_-_cropped.jpg"
                 alt="Giuseppe Petrocchi"
                 title="Giuseppe Petrocchi"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3f/ChristophePierre_%28cropped%29.jpg/250px-ChristophePierre_%28cropped%29.jpg"
                 alt="Christophe Louis Yves Georges Pierre"
                 title="Christophe Louis Yves Georges Pierre"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Mons._Pierbattista_Pizzaballa.jpg/250px-Mons._Pierbattista_Pizzaballa.jpg"
                 alt="Pierbattista Pizzaballa, O.F.M."
                 title="Pierbattista Pizzaballa, O.F.M."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Mario_Aurelio_Cardinal_Poli_%28cropped%29.JPG/250px-Mario_Aurelio_Cardinal_Poli_%28cropped%29.JPG"
                 alt="Mario Aurelio Poli"
                 title="Mario Aurelio Poli"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Poola_Anthony.jpg/250px-Poola_Anthony.jpg"
                 alt="Anthony Poola"
                 title="Anthony Poola"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7d/Mons._Robert_Prevost%2C_OSA_%28cropped%29.jpg/250px-Mons._Robert_Prevost%2C_OSA_%28cropped%29.jpg"
                 alt="Robert Francis Prevost, O.S.A."
                 title="Robert Francis Prevost, O.S.A."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8e/2025.02.22._Timothy_Radcliffe_OP_Warsaw_Poland_Photo_Mariusz_Kubik_01.JPG/250px-2025.02.22._Timothy_Radcliffe_OP_Warsaw_Poland_Photo_Mariusz_Kubik_01.JPG"
                 alt="Timothy Peter Joseph Radcliffe, O.P."
                 title="Timothy Peter Joseph Radcliffe, O.P."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d5/Baldassare_Reina.jpg/250px-Baldassare_Reina.jpg"
                 alt="Baldassare Reina"
                 title="Baldassare Reina"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f4/Coat_of_arms_of_Roberto_Repole_%28cardinal%29.svg/250px-Coat_of_arms_of_Roberto_Repole_%28cardinal%29.svg.png"
                 alt="Roberto Repole"
                 title="Roberto Repole"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7f/John_Ribat_%28cropped%29.jpg/250px-John_Ribat_%28cropped%29.jpg"
                 alt="John Ribat, M.S.C."
                 title="John Ribat, M.S.C."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/S%C3%A9rgio_da_Rocha.jpg/250px-S%C3%A9rgio_da_Rocha.jpg"
                 alt="Sérgio da Rocha"
                 title="Sérgio da Rocha"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ec/Bishop_Arthur_Roche_2008.jpg/250px-Bishop_Arthur_Roche_2008.jpg"
                 alt="Arthur Roche"
                 title="Arthur Roche"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/aa/Angel_Rossi_Arzobispo_1.1.jpg/250px-Angel_Rossi_Arzobispo_1.1.jpg"
                 alt="Ángel Sixto Rossi, S.J."
                 title="Ángel Sixto Rossi, S.J."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Escudo_de_Luis_Jos%C3%A9_Rueda_Aparicio.cardenal.svg/250px-Escudo_de_Luis_Jos%C3%A9_Rueda_Aparicio.cardenal.svg.png"
                 alt="Luis José Rueda Aparicio"
                 title="Luis José Rueda Aparicio"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/f/f5/Rugambwa_Protase.jpg"
                 alt="Protase Rugambwa"
                 title="Protase Rugambwa"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f5/Arcybiskup_Grzegorz_Ry%C5%9B_podczas_Wigilii_Paschalnej_2020_%28cropped%29.jpg/250px-Arcybiskup_Grzegorz_Ry%C5%9B_podczas_Wigilii_Paschalnej_2020_%28cropped%29.jpg"
                 alt="Grzegorz Wojciech Ryś"
                 title="Grzegorz Wojciech Ryś"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/57/Louis_Rapha%C3%ABl_I_Sako_November_2015.jpg/250px-Louis_Rapha%C3%ABl_I_Sako_November_2015.jpg"
                 alt="Louis Raphaël I Sako"
                 title="Louis Raphaël I Sako"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Semeraro_Udienza_20170504.jpg/250px-Semeraro_Udienza_20170504.jpg"
                 alt="Marcello Semeraro"
                 title="Marcello Semeraro"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/42/Archbishop_Berhaneyesus_Demerew_Souraphiel.jpg/250px-Archbishop_Berhaneyesus_Demerew_Souraphiel.jpg"
                 alt="Berhaneyesus Demerew Souraphiel, C.M."
                 title="Berhaneyesus Demerew Souraphiel, C.M."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fb/Dom_Jaime_Spengler_-_Arcebispo.JPG/250px-Dom_Jaime_Spengler_-_Arcebispo.JPG"
                 alt="Jaime Spengler, O.F.M."
                 title="Jaime Spengler, O.F.M."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/Leonardo_Steiner.jpg/250px-Leonardo_Steiner.jpg"
                 alt="Leonardo Ulrich Steiner, O.F.M."
                 title="Leonardo Ulrich Steiner, O.F.M."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/23/%2BIgnatius_Kardinal_Suharyo_%28cropped%29.jpg/250px-%2BIgnatius_Kardinal_Suharyo_%28cropped%29.jpg"
                 alt="Ignatius Suharyo Hardjoatmodjo"
                 title="Ignatius Suharyo Hardjoatmodjo"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/aa/Coat_of_arms_of_Orani_Jo%C3%A3o_Tempesta.svg/250px-Coat_of_arms_of_Orani_Jo%C3%A3o_Tempesta.svg.png"
                 alt="Orani João Tempesta, O. Cist."
                 title="Orani João Tempesta, O. Cist."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1e/Mgr._D%C3%A9sir%C3%A9_Tsarahazana.jpg/250px-Mgr._D%C3%A9sir%C3%A9_Tsarahazana.jpg"
                 alt="Désiré Tsarahazana"
                 title="Désiré Tsarahazana"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/30/Coat_of_arms_of_Emil_Paul_Tscherrig_%28cardinal%29.svg/250px-Coat_of_arms_of_Emil_Paul_Tscherrig_%28cardinal%29.svg.png"
                 alt="Emil Paul Tscherrig"
                 title="Emil Paul Tscherrig"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b0/Jean-Paul_Vesco%2C_OP-022022.jpg/250px-Jean-Paul_Vesco%2C_OP-022022.jpg"
                 alt="Jean-Paul Vesco, O.P."
                 title="Jean-Paul Vesco, O.P."
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3c/Card._Heung-sik.png/250px-Card._Heung-sik.png"
                 alt="Lazzaro You Heung-sik"
                 title="Lazzaro You Heung-sik"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/Mario_Zenari.jpg/250px-Mario_Zenari.jpg"
                 alt="Mario Zenari"
                 title="Mario Zenari"
                 data-bs-toggle="tooltip"
                 class="rounded border"
                 style="width:32px;height:32px;object-fit:cover;">
          
        </div>
      </div>
    

  </div>
</div>

<!-- Activate Bootstrap tooltips -->
<script>
document.addEventListener('DOMContentLoaded', () => {
  const tooltipList = [].slice.call(document.querySelectorAll('[data-bs-toggle="tooltip"]'));
  tooltipList.forEach(el => new bootstrap.Tooltip(el));
});
</script>
</div>

But perhaps more striking is where they’re from. They aren't all from Italy!

<div>
<div id="cardinal-map" class="w-100" style="position: relative;">
<div class="tooltip" style="display: none; position: absolute; background: white; padding: 4px 8px; border: 1px solid #ccc; pointer-events: none; font-size: 12px;"></div>
<svg preserveAspectRatio="xMidYMid meet" viewBox="0 0 960 600" class="w-100 h-auto d-block"></svg>
</div>


<style>
#cardinal-map circle {
    fill: #C41E3A;
    opacity: 0.7;
    stroke: #C41E3A;
    stroke-width: 0.5;
}
</style>

<script>
const data = [
{ country: "Philippines", count: 3, lat: 13.41, lon: 122.56 },
{ country: "Mexico", count: 2, lat: 23.63, lon: -102.55 },
{ country: "Portugal", count: 4, lat: 39.40, lon: -8.22 },
{ country: "Congo (Dem. Rep.)", count: 1, lat: -2.88, lon: 23.66 },
{ country: "Switzerland", count: 3, lat: 46.8, lon: 8.23 },
{ country: "Algeria", count: 1, lat: 28.03, lon: 1.66 },
{ country: "Brazil", count: 9, lat: -14.23, lon: -51.92 },
{ country: "Italy", count: 17, lat: 41.87, lon: 12.57 },
{ country: "Morocco", count: 2, lat: 31.79, lon: -7.09 },
{ country: "Côte d’Ivoire", count: 2, lat: 7.54, lon: -5.55 },
{ country: "Myanmar", count: 1, lat: 21.91, lon: 95.96 },
{ country: "Slovenia", count: 1, lat: 46.15, lon: 14.99 },
{ country: "Croatia", count: 1, lat: 45.10, lon: 15.20 },
{ country: "Nicaragua", count: 1, lat: 12.87, lon: -85.21 },
{ country: "South Africa", count: 1, lat: -30.56, lon: 22.94 },
{ country: "USA", count: 9, lat: 37.09, lon: -95.71 },
{ country: "Spain", count: 7, lat: 40.46, lon: -3.75 },
{ country: "Ukraine", count: 1, lat: 48.38, lon: 31.17 },
{ country: "Ecuador", count: 1, lat: -1.83, lon: -78.18 },
{ country: "Peru", count: 1, lat: -9.19, lon: -75.02 },
{ country: "Chile", count: 1, lat: -35.68, lon: -71.54 },
{ country: "China", count: 1, lat: 35.86, lon: 104.19 },
{ country: "Canada", count: 3, lat: 56.13, lon: -106.35 },
{ country: "India", count: 5, lat: 20.59, lon: 78.96 },
{ country: "Czech Republic", count: 1, lat: 49.82, lon: 15.47 },
{ country: "Belgium", count: 2, lat: 50.85, lon: 4.47 },
{ country: "New Zealand", count: 1, lat: -40.90, lon: 174.89 },
{ country: "Timor-Leste", count: 1, lat: -8.87, lon: 125.73 },
{ country: "Netherlands", count: 1, lat: 52.13, lon: 5.29 },
{ country: "Hungary", count: 1, lat: 47.16, lon: 19.50 },
{ country: "Ireland", count: 1, lat: 53.14, lon: -7.69 },
{ country: "Argentina", count: 3, lat: -38.42, lon: -63.62 },
{ country: "Malaysia", count: 1, lat: 4.21, lon: 101.98 },
{ country: "Cape Verde", count: 1, lat: 16.00, lon: -24.01 },
{ country: "Cuba", count: 1, lat: 21.52, lon: -77.78 },
{ country: "Singapore", count: 1, lat: 1.35, lon: 103.82 },
{ country: "Malta", count: 1, lat: 35.94, lon: 14.38 },
{ country: "Luxembourg", count: 1, lat: 49.82, lon: 6.13 },
{ country: "Rwanda", count: 1, lat: -1.94, lon: 29.87 },
{ country: "Japan", count: 2, lat: 36.20, lon: 138.25 },
{ country: "Thailand", count: 1, lat: 15.87, lon: 100.99 },
{ country: "Poland", count: 4, lat: 51.92, lon: 19.14 },
{ country: "Haïti", count: 1, lat: 18.97, lon: -72.29 },
{ country: "Tonga", count: 1, lat: -21.18, lon: -175.20 },
{ country: "Lithuania", count: 1, lat: 55.17, lon: 23.88 },
{ country: "Paraguay", count: 1, lat: -23.44, lon: -58.44 },
{ country: "Germany", count: 3, lat: 51.17, lon: 10.45 },
{ country: "South Sudan", count: 1, lat: 7.31, lon: 30.15 },
{ country: "Serbia", count: 1, lat: 44.02, lon: 21.00 },
{ country: "United Kingdom", count: 3, lat: 55.37, lon: -3.44 },
{ country: "Kenya", count: 1, lat: 0.02, lon: 37.91 },
{ country: "Central African Republic", count: 1, lat: 6.61, lon: 20.94 },
{ country: "Nigeria", count: 1, lat: 9.08, lon: 8.68 },
{ country: "Burkina Faso", count: 1, lat: 12.24, lon: -1.56 },
{ country: "Sri Lanka", count: 1, lat: 7.87, lon: 80.77 },
{ country: "France", count: 2, lat: 46.23, lon: 2.21 },
{ country: "Bosnia and Herzegovina", count: 1, lat: 43.92, lon: 17.67 },
{ country: "Guatemala", count: 1, lat: 15.78, lon: -90.23 },
{ country: "Papua New Guinea", count: 1, lat: -6.31, lon: 143.95 },
{ country: "Colombia", count: 1, lat: 4.57, lon: -74.30 },
{ country: "Tanzania", count: 1, lat: -6.37, lon: 34.89 },
{ country: "Iraq", count: 1, lat: 33.22, lon: 43.68 },
{ country: "Guinea", count: 1, lat: 9.95, lon: -9.70 },
{ country: "Ethiopia", count: 1, lat: 9.15, lon: 40.49 },
{ country: "Uruguay", count: 1, lat: -32.52, lon: -55.77 },
{ country: "Indonesia", count: 1, lat: -0.79, lon: 113.92 },
{ country: "Madagascar", count: 1, lat: -18.77, lon: 46.87 },
{ country: "Ghana", count: 1, lat: 7.95, lon: -1.02 },
{ country: "South Korea", count: 1, lat: 35.91, lon: 127.77 }
];

const width = 960, height = 600;
const svg = d3.select("#cardinal-map svg")
.attr("width", width)
.attr("height", height);

const projection = d3.geoNaturalEarth1()
.scale(160)
.translate([width / 2, height / 2]);

const path = d3.geoPath(projection);
const tooltip = d3.select("#cardinal-map .tooltip");

d3.json("https://unpkg.com/world-atlas@2.0.2/countries-110m.json").then(worldData => {
const countries = topojson.feature(worldData, worldData.objects.countries).features;

svg.append("g")
    .selectAll("path")
    .data(countries)
    .join("path")
    .attr("fill", "#e0e0e0")
    .attr("stroke", "#999")
    .attr("d", path);

svg.append("g")
    .selectAll("circle")
    .data(data)
    .join("circle")
    .attr("cx", d => projection([d.lon, d.lat])[0])
    .attr("cy", d => projection([d.lon, d.lat])[1])
    .attr("r", d => Math.sqrt(d.count) * 3)
    .on("mouseover", (event, d) => {
    tooltip.style("display", "block")
        .html(`<strong>${d.country}</strong><br>${d.count} cardinal(s)`)
        .style("left", (event.pageX + 10) + "px")
        .style("top", (event.pageY - 20) + "px");
    })
    .on("mouseout", () => tooltip.style("display", "none"));
});
</script>
</div>

Historically, conclaves were dominated by Europeans, especially Italians. Today, the electors come from every continent, reflecting the Church’s global shift. Africa, Asia, and Latin America now represent a growing share of the votes. 

<style>
  .charts-wrapper {
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
    max-width: 1200px;
    margin: 3rem auto;
  }
  .chart-box {
    flex: 1 1 45%;
    min-width: 300px;
    background: #fff;
    padding: 1rem;
  }
  .chart-title {
    text-align: center;
    font-weight: bold;
    margin-bottom: 0.5rem;
  }
  .chart-box svg {
    width: 100%;
    height: auto;
  }
</style>

<div class="charts-wrapper">
  <div class="chart-box">
    <div class="chart-title">By Continent</div>
    <div id="continent-bar"></div>
  </div>

  <div class="chart-box">
    <div class="chart-title">By Country</div>
    <div id="cardinal-bar"></div>
  </div>
</div>

{% raw %}
<script>
window.addEventListener("DOMContentLoaded", () => {
  if (typeof d3 === "undefined") return;

  // 1) Continent data
  const continentBarData = [
    { continent: "Europe",        count: 45 },
    { continent: "Asia",          count: 22 },
    { continent: "South America", count: 17 },
    { continent: "North America", count: 15 },
    { continent: "Africa",        count: 14 },
    { continent: "Oceania",       count: 4  }
  ];
  drawBarChart({
    selector: "#continent-bar",
    data: continentBarData,
    labelKey: "continent",
    valueKey: "count",
    margin: { top: 20, right: 20, bottom: 20, left: 100 }
  });

  // 2) Country data (full list)
  const countryBarData = [
    { country: "Italy",           count: 19 },
    { country: "USA",             count: 8  },
    { country: "Brazil",          count: 7  },
    { country: "Spain",           count: 7  },
    { country: "India",           count: 5  },
    { country: "Poland",          count: 4  },
    { country: "Portugal",        count: 4  },
    { country: "Germany",         count: 3  },
    { country: "United Kingdom",  count: 3  },
    { country: "Argentina",       count: 3  },
    { country: "Canada",          count: 3  },
    { country: "Philippines",     count: 3  },
    { country: "Switzerland",     count: 3  },
    { country: "Côte d’Ivoire",   count: 2  },
    { country: "Japan",           count: 2  },
    { country: "France",          count: 2  },
    { country: "Belgium",         count: 2  },
    { country: "Mexico",          count: 2  },
    { country: "Morocco",         count: 2  },
    { country: "Others",          count: 51 }
  ];
  drawBarChart({
    selector: "#cardinal-bar",
    data: countryBarData,
    labelKey: "country",
    valueKey: "count",
    margin: { top: 20, right: 20, bottom: 20, left: 140 }
  });

  // 3) Reusable drawer
  function drawBarChart({ selector, data, labelKey, valueKey, margin }) {
    const container = d3.select(selector);
    container.selectAll("*").remove();

    // parent-width for responsive sizing
    const fullWidth = container.node().getBoundingClientRect().width;
    const fullHeight = data.length * 28 + margin.top + margin.bottom;
    const width  = fullWidth - margin.left - margin.right;
    const height = fullHeight - margin.top - margin.bottom;

    const svg = container.append("svg")
      .attr("viewBox", `0 0 ${fullWidth} ${fullHeight}`)
      .append("g")
      .attr("transform", `translate(${margin.left},${margin.top})`);

    const x = d3.scaleLinear()
      .domain([0, d3.max(data, d => d[valueKey])])
      .range([0, width]);

    const y = d3.scaleBand()
      .domain(data.map(d => d[labelKey]))
      .range([0, height])
      .padding(0.15);

    svg.selectAll("rect")
      .data(data)
      .join("rect")
      .attr("y", d => y(d[labelKey]))
      .attr("width", d => x(d[valueKey]))
      .attr("height", y.bandwidth())
      .attr("fill", "#C41E3A");

    svg.append("g")
      .call(d3.axisLeft(y).tickSize(0))
      .selectAll("text")
      .style("font-size", "13px");

    svg.selectAll(".bar-label")
      .data(data)
      .join("text")
      .attr("class", "bar-label")
      .attr("x", d => x(d[valueKey]) + 6)
      .attr("y", d => y(d[labelKey]) + y.bandwidth()/2)
      .attr("dy", "0.35em")
      .text(d => d[valueKey])
      .style("font-size", "12px");
  }
});
</script>
{% endraw %}

The modern College of Cardinals is unmistakably global. While Europe (especially Italy) still holds the largest bloc, nearly half of all electors now hail from Asia, Africa, and the Americas combined. When the next conclave convenes, it won’t be a Euro-centric affair but a gathering that truly reflects the Church’s presence on every continent.

This widening geographic trend raises a fresh question: in such a diverse, worldwide Church, what qualities will cardinals value most in the next pontiff? Will they look for a seasoned Vatican insider—or a shepherd with feet on the ground across the globe?

## Shepherd or Statesman

<img src="/assets/images/posts/2025/05/2025-05-05-game-of-robes/section_2.png" alt="Shephred or Statesman" loading="lazy" width="100%" height="auto" />

Not all popes are the same. Some are theologians or power brokers. Others are parish priests at heart.

Historically, papal electors have leaned toward two kinds of leaders:
- Diplomatic popes: curial veterans, Rome insiders, administrators
- Pastoral popes: bishops and missionaries with deep local experience

Pope Francis, a former Jesuit and Archbishop of Buenos Aires, represents a clear shift toward the pastoral—emphasizing mercy, outreach, and simplicity. His style stands in contrast to Pope Benedict XVI, a theologian and doctrinal conservative rooted in the intellectual and liturgical traditions of the Church. Yet, not all popes fit neatly into one category. For example, John Paul II combined diplomatic weight as a global human rights advocate with a pastoral charisma that made him a beloved, globe-trotting figure.

<div>
  <div id="pope-grid" class="w-100" style="max-width: 700px; margin: 3rem auto;"></div>

  <script src="https://d3js.org/d3.v7.min.js"></script>
  {% raw %}
  <script>
  window.addEventListener("DOMContentLoaded", () => {
    const data = [
      { pope: "Pius X", x: -2, y: 5, dx: -10, dy: -20 },
      { pope: "Benedict XV", x: 3, y: 1, dx: 10, dy: 30 },
      { pope: "Pius XI", x: 2, y: 3, dx: 12, dy: -20 },
      { pope: "Pius XII", x: 4, y: 4, dx: 12, dy: -20 },
      { pope: "John XXIII", x: -1, y: -3, dx: -10, dy: 40 },
      { pope: "Paul VI", x: 1, y: -1, dx: 10, dy: 40 },
      { pope: "John Paul I", x: -3, y: -2, dx: -10, dy: 40 },
      { pope: "John Paul II", x: 1, y: 3, dx: 10, dy: -30 },
      { pope: "Benedict XVI", x: 4, y: 5, dx: 20, dy: -20 },
      { pope: "Francis", x: -4, y: -4, dx: -10, dy: 40 }
    ];

    const images = {
      "Pius X": "https://upload.wikimedia.org/wikipedia/commons/thumb/2/26/Pius_X%2C_by_Ernest_Walter_Histed_%28retouched%29.jpg/500px-Pius_X%2C_by_Ernest_Walter_Histed_%28retouched%29.jpg",
      "Benedict XV": "https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/Benedictus_XV%2C_by_Nicola_Perscheid%2C_1915_%28retouched%29.jpg/500px-Benedictus_XV%2C_by_Nicola_Perscheid%2C_1915_%28retouched%29.jpg",
      "Pius XI": "https://upload.wikimedia.org/wikipedia/commons/thumb/f/f9/Pius_XI%2C_by_Nicola_Perscheid_%28retouched%29.jpg/500px-Pius_XI%2C_by_Nicola_Perscheid_%28retouched%29.jpg",
      "Pius XII": "https://upload.wikimedia.org/wikipedia/commons/thumb/8/84/Pius_XII%2C_by_Michael_Pitcairn%2C_1951_%28cropped%29.jpg/500px-Pius_XII%2C_by_Michael_Pitcairn%2C_1951_%28cropped%29.jpg",
      "John XXIII": "https://upload.wikimedia.org/wikipedia/commons/thumb/0/0c/Ioannes_XXIII%2C_by_De_Agostini%2C_1958%E2%80%931963.jpg/500px-Ioannes_XXIII%2C_by_De_Agostini%2C_1958%E2%80%931963.jpg",
      "Paul VI": "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b0/Paulus_VI%2C_by_Fotografia_Felici%2C_1969.jpg/500px-Paulus_VI%2C_by_Fotografia_Felici%2C_1969.jpg",
      "John Paul I": "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Ioannes_Paulus_I%2C_by_Fotografia_Felici%2C_1978_%28cropped%29.jpg/500px-Ioannes_Paulus_I%2C_by_Fotografia_Felici%2C_1978_%28cropped%29.jpg",
      "John Paul II": "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/ADAMELLO_-_PAPA_-_Giovanni_Paolo_II_-_panoramio_%28cropped%29.jpg/500px-ADAMELLO_-_PAPA_-_Giovanni_Paolo_II_-_panoramio_%28cropped%29.jpg",
      "Benedict XVI": "https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/Benedykt_XVI_%282010-10-17%29_2.jpg/500px-Benedykt_XVI_%282010-10-17%29_2.jpg",
      "Francis": "https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Pope_Francis_Korea_Haemi_Castle_19.jpg/500px-Pope_Francis_Korea_Haemi_Castle_19.jpg"
    };

    const width = 720, height = 720, margin = 90;

    const svg = d3.select("#pope-grid")
      .append("svg")
      .attr("viewBox", `0 0 ${width} ${height}`)
      .attr("preserveAspectRatio", "xMidYMid meet");

    const x = d3.scaleLinear().domain([-5, 5]).range([margin, width - margin]);
    const y = d3.scaleLinear().domain([-5, 5]).range([height - margin, margin]);

    // Axes
    svg.append("line")
      .attr("x1", x(-5)).attr("x2", x(5))
      .attr("y1", y(0)).attr("y2", y(0))
      .attr("stroke", "#aaa");

    svg.append("line")
      .attr("x1", x(0)).attr("x2", x(0))
      .attr("y1", y(-5)).attr("y2", y(5))
      .attr("stroke", "#aaa");

    // Use HTML images overlaid via foreignObject to enable real CSS clipping
    svg.selectAll("foreignObject")
      .data(data)
      .join("foreignObject")
      .attr("x", d => x(d.x) - 20)
      .attr("y", d => y(d.y) - 20)
      .attr("width", 40)
      .attr("height", 40)
      .html(d => `
        <img src="${images[d.pope]}" width="40" height="40"
             style="border-radius: 50%; object-fit: cover;" />
      `);

    // Labels
    svg.selectAll("text.label")
      .data(data)
      .join("text")
      .attr("x", d => x(d.x) + d.dx)
      .attr("y", d => y(d.y) + d.dy)
      .text(d => d.pope)
      .attr("font-size", "13px")
      .attr("fill", "#333");

    // Axis labels
    svg.append("text")
      .attr("x", width / 2)
      .attr("y", height - 10)
      .attr("text-anchor", "middle")
      .text("Pastoral or Diplomatic")
      .attr("font-size", "14px");

    svg.append("text")
      .attr("transform", `rotate(-90)`)
      .attr("x", -height / 2)
      .attr("y", 20)
      .attr("text-anchor", "middle")
      .text("Reformist or Conservative")
      .attr("font-size", "14px");
  });
  </script>
  {% endraw %}
</div>

The matrix chart above maps modern popes along two key axes—*pastoral vs. diplomatic* and *reformist vs. conservative*—to highlight how their leadership styles and ideological priorities have shaped the Church. It reveals clear contrasts, from doctrinally strict diplomats like Benedict XVI to reform-minded pastors like Pope Francis.

Before the ballots are cast, each cardinal might weigh a candidate against these very traits, asking “Is he more shepherd or statesman? More reformer or conservative?” And "What does the church need today, and possibly for the next 30 years."

For all their differences, every pope—diplomat or pastor, reformer or conservative, has had to navigate the same secretive, super‐majority ballot process.

## The Sacred Numbers (Conclave Ballots)

<img src="/assets/images/posts/2025/05/2025-05-05-game-of-robes/section_3.png" alt="The Sacred Numbers (Conclave Ballots)" loading="lazy" width="100%" height="auto" />

The pope isn’t chosen by a single vote, they are chosen by many, in secret.

Each day of the conclave, cardinals cast up to four ballots. To win, a candidate must receive a two-thirds majority of votes—usually 80 out of 120. If no one reaches that threshold, voting continues until white smoke rises.

But not all votes are equal. Early rounds often feature [respectful votes for someone not expected to win](https://youtu.be/DNwgh787umM?si=NeEGitKxFOSOTWKe&t=357). Nods of esteem, not serious endorsements. As the rounds continue, names consolidate. Coalitions form. Surprises emerge.

Want to see how that played out in real life? The 2013 conclave began with Angelo Scola in the lead, but ended with a shock.

<div>
  <div id="ballot-stacked-chart" style="max-width: 800px; margin: 3rem auto;"></div>
  <script src="https://d3js.org/d3.v7.min.js"></script>
  <script>
  document.addEventListener("DOMContentLoaded", () => {
    const ballotData = [
      { ballot: 1, candidate: "Angelo Scola", votes: 30 },
      { ballot: 1, candidate: "Jorge Mario Bergoglio", votes: 26 },
      { ballot: 1, candidate: "Marc Ouellet", votes: 22 },
      { ballot: 1, candidate: "Seán Patrick O'Malley", votes: 10 },
      { ballot: 1, candidate: "Odilo Scherer", votes: 4 },
      { ballot: 1, candidate: "Others", votes: 23 },
      { ballot: 2, candidate: "Jorge Mario Bergoglio", votes: 45 },
      { ballot: 2, candidate: "Angelo Scola", votes: 38 },
      { ballot: 2, candidate: "Marc Ouellet", votes: 24 },
      { ballot: 2, candidate: "Others", votes: 8 },
      { ballot: 3, candidate: "Jorge Mario Bergoglio", votes: 56 },
      { ballot: 3, candidate: "Angelo Scola", votes: 41 },
      { ballot: 3, candidate: "Marc Ouellet", votes: 15 },
      { ballot: 3, candidate: "Others", votes: 3 },
      { ballot: 4, candidate: "Jorge Mario Bergoglio", votes: 67 },
      { ballot: 4, candidate: "Angelo Scola", votes: 32 },
      { ballot: 4, candidate: "Marc Ouellet", votes: 13 },
      { ballot: 4, candidate: "Others", votes: 3 },
      { ballot: 5, candidate: "Jorge Mario Bergoglio", votes: 85 },
      { ballot: 5, candidate: "Angelo Scola", votes: 20 },
      { ballot: 5, candidate: "Marc Ouellet", votes: 8 },
      { ballot: 5, candidate: "Others", votes: 2 }
    ];

    // pivot into one row per ballot
    const ballots = d3.groups(ballotData, d => d.ballot)
      .map(([b, rows]) => {
        const out = { ballot: `Ballot ${b}` };
        rows.forEach(r => out[r.candidate] = r.votes);
        return out;
      });

    // explicit bottom→top order
    const stackKeys = [
      "Others",
      "Odilo Scherer",
      "Seán Patrick O'Malley",
      "Marc Ouellet",
      "Angelo Scola",
      "Jorge Mario Bergoglio"
    ];

    const width  = 700;
    const height = 400;
    // bumped right margin to 240 to fit full legend text
    const margin = { top: 40, right: 240, bottom: 110, left: 70 };

    const svg = d3.select("#ballot-stacked-chart")
      .append("svg")
      .attr("viewBox", [
        0,
        0,
        width + margin.left + margin.right,
        height + margin.top + margin.bottom + 60
      ])
      .append("g")
      .attr("transform", `translate(${margin.left},${margin.top})`);

    const x = d3.scaleBand()
      .domain(ballots.map(d => d.ballot))
      .range([0, width])
      .padding(0.2);

    const y = d3.scaleLinear()
      .domain([0, 120])
      .range([height, 0]);

    const candidateColors = {
      "Jorge Mario Bergoglio": "#FFF200",
      "Angelo Scola":           "#C41E3A",
      "Marc Ouellet":           "#8B182B",
      "Seán Patrick O'Malley":  "#E87D7D",
      "Odilo Scherer":          "#660000",
      "Others":                 "#D3D3D3"
    };

    const color = d3.scaleOrdinal()
      .domain(stackKeys)
      .range(stackKeys.map(k => candidateColors[k]));

    const stackedData = d3.stack().keys(stackKeys)(ballots);

    // draw bars
    svg.append("g")
      .selectAll("g")
      .data(stackedData)
      .join("g")
      .attr("fill", d => color(d.key))
      .selectAll("rect")
      .data(d => d)
      .join("rect")
        .attr("x", d => x(d.data.ballot))
        .attr("y", d => y(d[1]))
        .attr("height", d => y(d[0]) - y(d[1]))
        .attr("width", x.bandwidth());

    // x-axis
    svg.append("g")
      .attr("transform", `translate(0,${height})`)
      .call(d3.axisBottom(x))
      .selectAll("text")
      .attr("font-size", "14px");

    // y-axis
    svg.append("g")
      .call(d3.axisLeft(y).ticks(6))
      .selectAll("text")
      .attr("font-size", "14px");

    // title
    svg.append("text")
      .attr("x", width / 2).attr("y", -10)
      .attr("text-anchor", "middle")
      .attr("font-size", "18px")
      .text("2013 Papal Conclave – Ballot Voting Shifts");

    // x-label
    svg.append("text")
      .attr("x", width / 2).attr("y", height + 50)
      .attr("text-anchor", "middle")
      .attr("font-size", "14px")
      .text("Ballot Number");

    // y-label
    svg.append("text")
      .attr("transform", "rotate(-90)")
      .attr("x", -height / 2).attr("y", -50)
      .attr("text-anchor", "middle")
      .attr("font-size", "14px")
      .text("Votes Received");

    // legend on the right
    const legend = svg.append("g")
      .attr("transform", `translate(${width + 20}, 0)`);

    // render legend entries top-down
    stackKeys.slice().reverse().forEach((name, i) => {
      const g = legend.append("g")
        .attr("transform", `translate(0, ${i * 24})`);
      g.append("rect")
        .attr("width", 16)
        .attr("height", 16)
        .attr("fill", color(name))
        .attr("stroke", name === "Jorge Mario Bergoglio" ? "#000" : "none")
        .attr("stroke-width", name === "Jorge Mario Bergoglio" ? 2 : 0);
      g.append("text")
        .attr("x", 22).attr("y", 12)
        .attr("font-size", "13px")
        .attr("fill", "#333")
        .text(name);
    });
  });
  </script>
</div>

See how the 2013 conclave shifted from frontrunners like Angelo Scola to an unexpected consensus: Jorge Mario Bergoglio, elected Pope Francis.

Yet if you’d glanced at the betting markets on Day 1, you’d never have guessed Bergoglio’s surge. Odds on the favorite barely budged even as votes swung—and that tells its own story about uncertainty in the Sistine Chapel.

## Betrayed by the Odds

<img src="/assets/images/posts/2025/05/2025-05-05-game-of-robes/section_4.png" alt="Betrayed by the Odds" loading="lazy" width="100%" height="auto" />

If you had asked the betting markets in 2013, the next pope was going to be Angelo Scola. Or maybe Peter Turkson. Or Marc Ouellet. Jorge Mario Bergoglio? A longshot, barely on the board.

Right now the betting market Polymarket has Parolin, Tagle, and Zuppi favored to win.

<div class="mb-5">
<iframe
  src="https://embed.polymarket.com/market.html?market=will-pietro-parolin-be-the-next-pope&features=volume&theme=light"
  title="Polymarket: Will Pietro Parolin be the next Pope?"
  allowfullscreen
  style="border:0; width:100%; height:100%;">
</iframe>

<iframe
  src="https://embed.polymarket.com/market.html?market=will-luis-antonio-tagle-be-the-next-pope&features=volume&theme=light"
  title="Polymarket: Will Pietro Parolin be the next Pope?"
  allowfullscreen
  style="border:0; width:100%; height:100%;">
</iframe>

<iframe
  src="https://embed.polymarket.com/market.html?market=will-matteo-zuppi-be-the-next-pope&features=volume&theme=light"
  title="Polymarket: Will Pietro Parolin be the next Pope?"
  allowfullscreen
  style="border:0; width:100%; height:100%;">
</iframe>
</div>


Despite all the speculation, the markets in 2013 completely missed it. Even when leaks began to surface midway through the conclave, odds barely moved in favor of Bergoglio! To see exactly when (and if) the odds caught up to the cardinals’ ballots, we pulled hourly implied probabilities for the three front-runners and mapped them against six key conclave milestones.

<div class="py-5">
<div id="odds-race" style="max-width: 800px; margin: 3rem auto;"></div>
<button id="start-race" style="display: block; margin: 0 auto 2rem auto;">Similate 2013 Betting Markets!</button>

<script src="https://d3js.org/d3.v7.min.js"></script>
<script>
document.addEventListener("DOMContentLoaded", () => {
  const bettingRaceData = [
    { Event: "Pre-Conclave", Bergoglio: 0.02, Ouellet: 0.40, Scola: 0.50 },
    { Event: "Black Smoke 1", Bergoglio: 0.01, Ouellet: 0.38, Scola: 0.50 },
    { Event: "La Stampa 1", Bergoglio: 0.01, Ouellet: 0.38, Scola: 0.50 },
    { Event: "Black Smoke 2", Bergoglio: 0.03, Ouellet: 0.48, Scola: 0.55 },
    { Event: "La Stampa 2", Bergoglio: 0.03, Ouellet: 0.48, Scola: 0.53 },
    { Event: "La Stampa 2 (t-1)", Bergoglio: 0.18, Ouellet: 0.42, Scola: 0.55 },
    { Event: "Final Smoke", Bergoglio: 0.30, Ouellet: 0.35, Scola: 0.50 },
    { Event: "Election", Bergoglio: 0.60, Ouellet: 0.35, Scola: 0.10 },
    { Event: "Final Results", Bergoglio: 0.60, Ouellet: 0.35, Scola: 0.10 }
  ];

  const candidates = ["Bergoglio", "Ouellet", "Scola"];
  const margin = { top: 40, right: 20, bottom: 100, left: 80 };
  const width = 800 - margin.left - margin.right;
  const height = 400 - margin.top - margin.bottom;

  const svg = d3.select("#odds-race")
    .append("svg")
    .attr("viewBox", [0, 0, width + margin.left + margin.right, height + margin.top + margin.bottom])
    .append("g")
    .attr("transform", `translate(${margin.left},${margin.top})`);

  const x = d3.scalePoint()
    .domain(bettingRaceData.map(d => d.Event))
    .range([0, width])
    .padding(0.5);

  const y = d3.scaleLinear()
    .domain([0, d3.max(candidates, c => d3.max(bettingRaceData, d => d[c]))])
    .nice()
    .range([height, 0]);

  svg.append("text")
    .attr("x", width / 2)
    .attr("y", -10)
    .attr("text-anchor", "middle")
    .attr("font-size", "16px")
    .text("Betting Market Odds – Papal Conclave 2013");

  svg.append("g")
    .attr("transform", `translate(0,${height})`)
    .call(d3.axisBottom(x))
    .selectAll("text")
    .attr("transform", "rotate(-40)")
    .attr("dx", "-0.8em")
    .attr("dy", "0.15em")
    .style("text-anchor", "end");

  svg.append("g").call(d3.axisLeft(y));

  const color = d3.scaleOrdinal()
    .domain(candidates)
    .range(["#007bff", "#28a745", "#dc3545"]);

  const lines = candidates.map(name => {
    const lineData = bettingRaceData.map((d, i) => ({
      x: x(d.Event) + (d.Event === "Post-Election" ? 10 : 0), // slight shift for post-election
      y: y(d[name])
    }));

    const line = svg.append("path")
      .datum(lineData)
      .attr("fill", "none")
      .attr("stroke", color(name))
      .attr("stroke-width", 2)
      .attr("d", d3.line()
        .x(d => d.x)
        .y(d => d.y)
        .curve(d3.curveStepAfter)
      );

    const totalLength = line.node().getTotalLength();

    line
      .attr("stroke-dasharray", `${totalLength} ${totalLength}`)
      .attr("stroke-dashoffset", totalLength);

    return { name, line };
  });

  svg.append("text")
    .attr("x", width / 2)
    .attr("y", height + 80)
    .attr("text-anchor", "middle")
    .attr("font-size", "14px")
    .text("Key Conclave Event Sequence");

  svg.append("text")
    .attr("transform", "rotate(-90)")
    .attr("y", -60)
    .attr("x", -height / 2)
    .attr("text-anchor", "middle")
    .attr("font-size", "14px")
    .text("Implied Odds of Becoming Pope");

  const legend = svg.append("g")
    .attr("transform", `translate(${width - 120}, 10)`);

  candidates.forEach((name, i) => {
    const g = legend.append("g").attr("transform", `translate(0, ${i * 20})`);
    g.append("rect")
      .attr("width", 12)
      .attr("height", 12)
      .attr("fill", color(name));
    g.append("text")
      .attr("x", 16)
      .attr("y", 10)
      .text(name)
      .attr("font-size", "12px")
      .attr("fill", "#000");
  });

  document.getElementById("start-race").addEventListener("click", () => {
    lines.forEach(({ line }) => {
      const totalLength = line.node().getTotalLength();
      line.transition()
        .duration(5000)
        .ease(d3.easeLinear)
        .attr("stroke-dashoffset", 0);
    });
  });
});
</script>
</div>

Reading the chart:
- Early flatlines show Scola (red) as the overwhelming favorite, with Ouellet (green) in second and Bergoglio (blue) barely registering.
- After Black Smoke 2, Bergoglio’s blue line breaks upward—our model picks up the first real vote momentum.
- A second leak (La Stampa 2) accelerates his ascent even more, while Scola and Ouellet plateau or dip.
- Only by Final Smoke does the blue line overtake the others, matching the cardinals’ eventual choice.

Because these curves were made using reconstructed data not pulled straight from Betfair data, they’re best read as a relative guide to market movement rather than exact odds. They still give a clear visual of how late the market caught Francis’s surge. You can read more about forecasting closed-door decisions [in this paper](https://nottingham-repository.worktribe.com/output/1421334/forecasting-the-outcome-of-closed-door-decisions-evidence-from-500-years-of-betting-on-papal-conclaves).

With the chart’s simulated odds laid bare, showing just how slow the markets were to catch Francis’s rise, we’re left with a simple question: if even a liquid betting market can miss the signals, could a dedicated model do better?

## Can We Predict the Next Pope?

<img src="/assets/images/posts/2025/05/2025-05-05-game-of-robes/section_5.png" alt="Can We Predict the Next Pope" loading="lazy" width="100%" height="auto" />

No one really knows who the next pope will be. The conclave is secret. The alliances are subtle. And divine inspiration doesn’t leave a paper trail.

But we tried anyway.

We built a simple, interactive model that lets you dial in what you believe matters:
- Political leaning: Choose Neutral, Conservative, or Reformist to boost cardinals on that spectrum
- Curia insider bias: Decide whether Vatican insiders get an edge
- Geography: Favor Italian candidates, global voices, or remain neutral
- Age: Toggle a preference for cardinals under 75
- Francis’s picks: Give extra weight to those closely aligned with Pope Francis
- Polymarket odds: Add a market-based nudge in favor of front-runners (data as of May 6, 2025)
- Real-time scoring: See exactly how each factor contributes when you check “Show factors”

<div>
<div class="container py-4">
  <style>
  input[type="radio"],
  input[type="checkbox"] {
    accent-color: #C41E3A !important;
  }

  :root {
    --bs-form-check-input-checked-bg: #C41E3A;
    --bs-form-check-input-checked-border-color: #C41E3A;
  }

  .form-check-input:checked {
    background-color: var(--bs-form-check-input-checked-bg) !important;
    border-color: var(--bs-form-check-input-checked-border-color) !important;
  }

  .form-check-input:focus {
    box-shadow: 0 0 0 0.25rem rgba(196,29,58,.25) !important;
  }
</style>

  <div class="row">

    <!-- Controls -->
    <div class="col-md-4">
      <h5>Conclave Criteria</h5>

      <!-- 1) Political Leaning -->
      <div class="mb-3">
        <label class="form-label">Political Leaning</label><br>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="lean" id="lean-neutral"   value="neutral" checked>
          <label class="form-check-label" for="lean-neutral">Neutral</label>
        </div>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="lean" id="lean-conservative" value="conservative">
          <label class="form-check-label" for="lean-conservative">Conservative</label>
        </div>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="lean" id="lean-reformist"   value="reformist">
          <label class="form-check-label" for="lean-reformist">Reformist</label>
        </div>
      </div>

      <!-- 2) Curia Insiders -->
      <div class="mb-3">
        <label class="form-label">Curia Insiders</label><br>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="curia" id="curia-no"  value="no" checked>
          <label class="form-check-label" for="curia-no">Neutral</label>
        </div>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="curia" id="curia-yes" value="yes">
          <label class="form-check-label" for="curia-yes">Favor</label>
        </div>
      </div>

      <!-- 3) Geography -->
      <div class="mb-3">
        <label class="form-label">Geography</label><br>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="geo" id="geo-neutral" value="neutral" checked>
          <label class="form-check-label" for="geo-neutral">Neutral</label>
        </div>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="geo" id="geo-italy"   value="italy">
          <label class="form-check-label" for="geo-italy">Favor Italy</label>
        </div>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="geo" id="geo-global"  value="global">
          <label class="form-check-label" for="geo-global">Favor Global</label>
        </div>
      </div>

      <!-- 4) Under-75 -->
      <div class="mb-3">
        <label class="form-label">Under-75</label><br>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="under75" id="u75-no" value="no" checked>
          <label class="form-check-label" for="u75-no">Neutral</label>
        </div>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="under75" id="u75-yes" value="yes">
          <label class="form-check-label" for="u75-yes">Favor</label>
        </div>
      </div>

      <!-- 5) Francis Picks -->
      <div class="mb-3">
        <label class="form-label">Francis Picks</label><br>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="francis" id="fr-no" value="no" checked>
          <label class="form-check-label" for="fr-no">Neutral</label>
        </div>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="francis" id="fr-yes" value="yes">
          <label class="form-check-label" for="fr-yes">Favor</label>
        </div>
      </div>

      <!-- 6) Polymarket Odds -->
      <div class="mb-3 form-check">
        <input class="form-check-input" type="checkbox" id="polymarket" checked>
        <label class="form-check-label" for="polymarket">Favor Polymarket Odds</label>
      </div>

      <!-- 7) Show Factors -->
      <div class="mb-3 form-check">
        <input class="form-check-input" type="checkbox" id="show-factors">
        <label class="form-check-label" for="show-factors">Show factors</label>
      </div>
    </div>

    <!-- Winner Display -->
    <div class="col-md-8 text-center">
      <h5>Selected Winner</h5>
      <div id="winner-card" class="card mx-auto position-relative" style="width: 18rem;">
        <img id="winner-img" class="card-img-top" src="" alt="Winner">
        <div class="card-body">
          <h5 id="winner-name" class="card-title"></h5>
          <p id="winner-title" class="card-text"></p>
          <pre id="winner-factors" 
               style="font-family: monospace; color: grey; display: none; margin-top: 0.5rem;">
          </pre>
        </div>
        <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/Sede_vacante.svg/380px-Sede_vacante.svg.png"
             style="width: 40px; position: absolute; top: 10px; right: 10px;" alt="Papal Regalia">
      </div>
    </div>

  </div>
</div>

<script src="https://d3js.org/d3.v7.min.js"></script>
<script>
  //── Candidate data ─────────────────────────────────────────────────────────
  const candidates = [
    { name:"Pietro Parolin",         img:"https://upload.wikimedia.org/wikipedia/commons/thumb/0/00/Cardinal_Pietro_Parolin_par_Claude_Truong-Ngoc_juillet_2021.jpg/500px-Cardinal_Pietro_Parolin_par_Claude_Truong-Ngoc_juillet_2021.jpg",
      conservative:0, reformist:0, curia:2, local:2, under75:1, francis:2, polymarket:27, title:"Secretary Emeritus"
    },
    { name:"Luis A. Tagle",          img:"https://upload.wikimedia.org/wikipedia/commons/thumb/6/60/Luis_Antonio_Tagle_at_Manila_in_2016_%28cropped%29.jpg/500px-Luis_Antonio_Tagle_at_Manila_in_2016_%28cropped%29.jpg",
      conservative:0, reformist:2, curia:0, local:1, under75:1, francis:1, polymarket:23, title:"Pro-Prefect Emeritus"
    },
    { name:"Matteo Maria Zuppi",     img:"https://upload.wikimedia.org/wikipedia/commons/thumb/8/8f/Matteo_Zuppi_2019.jpg/500px-Matteo_Zuppi_2019.jpg",
      conservative:0, reformist:2, curia:0, local:2, under75:1, francis:2, polymarket:10, title:"Archbishop of Bologna"
    },
    { name:"Peter Turkson",          img:"https://upload.wikimedia.org/wikipedia/commons/thumb/3/32/Peter_Turkson_%28cropped%29.jpg/500px-Peter_Turkson_%28cropped%29.jpg",
      conservative:0, reformist:0, curia:0, local:0, under75:0, francis:0, polymarket:5,  title:"Chancellor Pont. Academies"
    },
    { name:"Christoph Schönborn",    img:"https://upload.wikimedia.org/wikipedia/commons/d/da/Sch%C3%B6nborn_%C3%96sterreichische_Bischofskonferenz_20200616.jpg",
      conservative:2, reformist:0, curia:0, local:0, under75:0, francis:0, polymarket:1,  title:"Archbishop of Vienna"
    },
    { name:"Marc Ouellet",           img:"https://upload.wikimedia.org/wikipedia/commons/9/9a/Marc_Ouellet_%28cropped%29.jpg",
      conservative:2, reformist:0, curia:2, local:0, under75:0, francis:0, polymarket:1,  title:"Prefect Emeritus of Bishops"
    },
    { name:"Angelo Scola",           img:"https://upload.wikimedia.org/wikipedia/commons/thumb/6/6d/Kardinal_Woelki_Begruessungsempfang_Rathaus_2014-09-28_11.jpg/500px-Kardinal_Woelki_Begruessungsempfang_Rathaus_2014-09-28_11.jpg",
      conservative:2, reformist:0, curia:0, local:2, under75:0, francis:0, polymarket:1,  title:"Archbishop Emeritus of Milan"
    },
    { name:"Robert Sarah",           img:"https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/Cardinal_Robert_Sarah_%28cropped%29.JPG/500px-Cardinal_Robert_Sarah_%28cropped%29.JPG",
      conservative:2, reformist:0, curia:2, local:0, under75:0, francis:0, polymarket:2,  title:"Prefect Emeritus (Div. Worship)"
    },
    { name:"Leonardo Sandri",        img:"https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Argentine_Cardinal_Leonardo_Sandri_in_2014_%28cropped%29.jpg/500px-Argentine_Cardinal_Leonardo_Sandri_in_2014_%28cropped%29.jpg",
      conservative:0, reformist:0, curia:2, local:0, under75:0, francis:0, polymarket:1,  title:"Prefect Emeritus (Eastern Ch.)"
    },
    { name:"Gianfranco Ravasi",      img:"https://upload.wikimedia.org/wikipedia/commons/thumb/7/7b/Gianfranco_Ravasi_2018_%28cropped%29.jpg/500px-Gianfranco_Ravasi_2018_%28cropped%29.jpg",
      conservative:0, reformist:0, curia:0, local:2, under75:0, francis:0, polymarket:1,  title:"President Emeritus (Culture)"
    },
    { name:"Pierbattista Pizzaballa",img:"https://upload.wikimedia.org/wikipedia/commons/b/bf/Mons._Pierbattista_Pizzaballa.jpg",
      conservative:0, reformist:0, curia:0, local:2, under75:1, francis:2, polymarket:9,  title:"Patriarch of Jerusalem"
    },
    { name:"Péter Erdő",             img:"https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/P%C3%A9ter_Erd%C5%91_in_2011.jpg/500px-P%C3%A9ter_Erd%C5%91_in_2011.jpg",
      conservative:2, reformist:0, curia:0, local:0, under75:1, francis:0, polymarket:7,  title:"Archbishop of Esztergom"
    },
    { name:"Robert F. Prevost",      img:"https://upload.wikimedia.org/wikipedia/commons/7/7d/Mons._Robert_Prevost%2C_OSA_%28cropped%29.jpg",
      conservative:0, reformist:0, curia:2, local:0, under75:1, francis:2, polymarket:1,  title:"Prefect of Bishops"
    },
    { name:"José T. de Mendonça",    img:"https://upload.wikimedia.org/wikipedia/commons/thumb/0/09/Cardeal_De_Mendon%C3%A7a.jpg/500px-Cardeal_De_Mendon%C3%A7a.jpg",
      conservative:0, reformist:2, curia:0, local:0, under75:1, francis:2, polymarket:2,  title:"Prefect (Cult. & Educ.)"
    },
    { name:"Jean-Marc Aveline",      img:"https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/Mgr_Jean-Marc_Aveline_par_Claude_Truong-Ngoc_juillet_2021.jpg/500px-Mgr_Jean-Marc_Aveline_par_Claude_Truong-Ngoc_juillet_2021.jpg",
      conservative:0, reformist:0, curia:0, local:0, under75:1, francis:2, polymarket:5,  title:"Archbishop of Marseille"
    },
    { name:"Mario Grech",            img:"https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Cardinal_Mario_Grech_%28cropped%29.jpg/500px-Cardinal_Mario_Grech_%28cropped%29.jpg",
      conservative:0, reformist:2, curia:2, local:0, under75:1, francis:2, polymarket:2,  title:"Secretary Gen. of Synod"
    },
    { name:"Fridolin Ambongo Besungu",       
      img:"https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/AED_11%C3%A8me_Nuit_des_T%C3%A9moins_-_Fridolin_Ambongo_-_2_%28cropped%29.jpg/500px-AED_11%C3%A8me_Nuit_des_T%C3%A9moins_-_Fridolin_Ambongo_-_2_%28cropped%29.jpg",
      conservative:0, reformist:2, curia:0, local:0, under75:1, francis:2, polymarket:2,  title:"Archbishop of Kinshasa"
    }
  ];

  function updateWinner() {
    const W = {
      lean:       2,
      curia:      1.5,
      geo:        3,
      under75:    1.5,
      francis:    1.5,
      polymarket: 0.5
    };

    // read controls
    const leanSel  = document.querySelector('input[name="lean"]:checked').value;
    const curiaSel = document.querySelector('input[name="curia"]:checked').value;
    const geoSel   = document.querySelector('input[name="geo"]:checked').value;
    const u75Sel   = document.querySelector('input[name="under75"]:checked').value;
    const frSel    = document.querySelector('input[name="francis"]:checked').value;
    const polyOn   = document.getElementById('polymarket').checked;
    const showFac  = document.getElementById('show-factors').checked;

    candidates.forEach(c => {
      let s = 0, details = [];
      // political
      if (leanSel === 'conservative' && c.conservative) {
        const v = c.conservative * W.lean; s += v;
        details.push(`Conservative ×${c.conservative} ⇒ ${v.toFixed(1)}`);
      }
      if (leanSel === 'reformist' && c.reformist) {
        const v = c.reformist * W.lean; s += v;
        details.push(`Reformist ×${c.reformist} ⇒ ${v.toFixed(1)}`);
      }
      // curia
      if (curiaSel === 'yes' && c.curia) {
        const v = c.curia * W.curia; s += v;
        details.push(`Curia ×${c.curia} ⇒ ${v.toFixed(1)}`);
      }
      // geography
      if (geoSel === 'italy') {
        const v = c.local * W.geo; s += v;
        details.push(`Italy ×${c.local} ⇒ ${v.toFixed(1)}`);
      } else if (geoSel === 'global') {
        const inv = (2 - c.local);
        const v   = inv * W.geo; s += v;
        details.push(`Global ×${inv} ⇒ ${v.toFixed(1)}`);
      }
      // under75
      if (u75Sel === 'yes' && c.under75) {
        const v = c.under75 * W.under75; s += v;
        details.push(`Under-75 ×${c.under75} ⇒ ${v.toFixed(1)}`);
      }
      // francis
      if (frSel === 'yes' && c.francis) {
        const v = c.francis * W.francis; s += v;
        details.push(`Francis ×${c.francis} ⇒ ${v.toFixed(1)}`);
      }
      // polymarket
      if (polyOn && c.polymarket) {
        const v = c.polymarket * W.polymarket; s += v;
        details.push(`Polymarket ×${c.polymarket} ⇒ ${v.toFixed(1)}`);
      }

      c.score   = s;
      c.details = details;
    });

    // pick top
    const winner = candidates.sort((a,b) => b.score - a.score)[0];

    // render
    d3.select('#winner-name').text(winner.name);
    d3.select('#winner-title').text(winner.title);
    d3.select('#winner-img').attr('src', winner.img);

    // show/hide factors
    const pf = d3.select('#winner-factors');
    if (showFac) {
      pf.style('display','block')
        .text(winner.details.join('\n'));
    } else {
      pf.style('display','none');
    }
  }

  // bind & init
  d3.selectAll('input').on('change', updateWinner);
  updateWinner();
</script>

</div>

As you can see if you play around with the simplistic model, there are some definite favorites. But will this be the Pope? 

There's no good way to tell. Reliable historical data isn't available because of the secrecy of conclaves and the betting markets are unreliable. Almost certainly, based on precedent, the pope will be one of the Catholic Cardinals. The last pope to not be was [Pope Urban VI](https://en.wikipedia.org/wiki/Pope_Urban_VI).

No matter how many sliders you tweak or odds you consult, the truth remains the same: until the cardinals file into the Sistine Chapel and the smoke curls into the sky, every forecast is just an educated guess.

## Conclusion

In the end, there’s really no good way to figure out who the Pope might be before the conclave convenes to decide. You just kind of have to wait. And I wouldn't recommend betting on it. The smoke will rise, the name will be announced, and the world will meet a new pope. The Pope will be chosen not by campaign, but by consensus, prayer, and maybe a little politics.

We can analyze the data, trace the history, and watch the odds shift. But the final answer still comes in the form of white smoke rising over St. Peter’s Square.

Until then, the Game of Robes continues...

<img src="/assets/images/posts/2025/05/2025-05-05-game-of-robes/footer.png" alt="Game of Robes Footer" loading="lazy" width="100%" height="auto" style="opacity: 0.75;" />

<hr>

<style>
  .bibliography, 
  .bibliography * {
    font-size: 0.6rem !important;
    line-height: 1.2 !important;
  }
  .bibliography h2 {
    margin-top: 0.5rem;
    margin-bottom: 0.5rem;
  }
  .bibliography ul {
    margin-top: 0;
    padding-left: 1rem;
  }
</style>

<div class="bibliography">
  <h2>Methods</h2>
  <ul>
    <li>Scraped voting-age cardinal data from Wikipedia</li>
    <li>Validated dates, orders, and procedures via Catholic-Hierarchy, CSUN Conclave List, and FIU Cardinals Catalogs</li>
    <li>Built an interactive model with political, institutional, geographic, age, papal-pick, and market-odds inputs</li>
  </ul>

  <h2>Bibliography</h2>
  <ul>
    <li>https://en.wikipedia.org/wiki/List_of_cardinals</li>
    <li>https://catholic-hierarchy.org/</li>
    <li>https://www.csun.edu/~hcfll004/conclave-list.html</li>
    <li>https://cardinals.fiu.edu/catalogs.htm</li>
  </ul>
</div>