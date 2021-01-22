<script>
  import { onMount } from "svelte";

  let vow_colors = null;
  let seduced_colors = null;

  let container;
  let vow_svg;
  let sed_svg;

  const square_side = 10;
  const spacing = 0;
  const svg_margin = 6;

  function resize() {
    const w = container.node().getBoundingClientRect().width;
    render((w - svg_margin) / 2);
  }

  function render(w) {
    let row = Math.floor(w / (square_side + spacing));

    let vow_rect = vow_svg.selectAll("rect");
    let sed_rect = sed_svg.selectAll("rect");

    let h =
      Math.ceil(Math.max(vow_rect.size(), sed_rect.size()) / row) *
      (square_side + spacing);

    vow_svg.attr("width", w).attr("height", h);
    sed_svg.attr("width", w).attr("height", h);

    vow_rect
      .attr("x", function (d) {
        return (d.frame % row) * (square_side + spacing);
      })
      .attr("y", function (d) {
        return Math.floor(d.frame / row) * (square_side + spacing);
      });

    sed_rect
      .attr("x", function (d) {
        return (d.frame % row) * (square_side + spacing);
      })
      .attr("y", function (d) {
        return Math.floor(d.frame / row) * (square_side + spacing);
      });
  }

  function init() {
    d3.csv("data/dec-09-20-nxivm/Vow_Frames.csv", function (data) {
      data = data.slice(0, 2087); // Cutting off bumper frames
      d3.select("#colors_vow svg")
        .selectAll()
        .data(data)
        .enter()
        .append("rect")
        .attr("width", square_side)
        .attr("height", square_side)
        .style("fill", function (d) {
          return d.hex;
        });
      resize();
    });

    d3.csv("data/dec-09-20-nxivm/Seduced_Frames.csv", function (data) {
      data = data.slice(0, 644); // Cutting off bumper frames
      d3.select("#colors_seduced svg")
        .selectAll()
        .data(data)
        .enter()
        .append("rect")
        .attr("width", square_side)
        .attr("height", square_side)
        .style("fill", function (d) {
          return d.hex;
        });
      resize();
    });

    window.addEventListener("resize", resize);
  }

  onMount(() => {
    container = d3.select("#colors_container");
    vow_svg = d3.select("#colors_vow svg");
    sed_svg = d3.select("#colors_seduced svg");

    init();
  });
</script>

<div class="horiz_flex" id="colors_container">
  <div id="colors_vow">
    <div class="colors_title">
      <a href="https://www.youtube.com/watch?v=eXKJyh_rl0I" target="_blank"
        >The Vow</a
      >
    </div>
    <svg />

    <div class="colors_avg">
      <div class="colors_title">Average Color of The Vow Trailer</div>
      <div class="colors_avg_box" style="background-color:#544e4e" />
    </div>
  </div>

  <div id="colors_seduced">
    <div class="colors_title">
      <a href="https://www.youtube.com/watch?v=ch_4jn_m6-g" target="_blank"
        >Seduced</a
      >
    </div>
    <svg />
    <div class="colors_avg">
      <div class="colors_title">Average Color of Seduced Trailer</div>
      <div class="colors_avg_box" style="background-color:#282927" />
    </div>
  </div>
</div>

<style>
  .colors_avg {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 10px;
  }

  .colors_avg_box {
    width: 40px;
    height: 40px;
  }

  .colors_title,
  .colors_title a {
    font-size: 1.2rem;
    text-align: center;
    font-weight: 600;
    margin: 1em 0;
  }
</style>
