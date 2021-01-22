<script>
  import { onMount } from "svelte";

  let lacusta_sr_svg;
  let norm2_sr_svg;

  var light = "#f7f7f7";
  var lacusta_color = "#6930c3";
  var norm_color = "#5390d9";
  var bar_opacity = 0.8;
  var side = 300;
  var rad_margin = {
      top: 20,
      right: 0,
      bottom: 0,
      left: 0,
    },
    width = side - rad_margin.left - rad_margin.right,
    height = side - rad_margin.top - rad_margin.bottom,
    innerRadius = 80,
    outerRadius = Math.min(width, height) / 2;

  var users = [
    "Max Blumenthal",
    "Mnar Muhawesh Adley",
    "Noah C Rothman",
    "WRPitt",
  ];
  var currentUser = users[0];

  var user_descriptions = {
    "Max Blumenthal":
      '<a href="https://twitter.com/maxblumenthal?lang=en" target="_blank">@MaxBlumenthal</a> is an editor of <a href="https://thegrayzone.com/" target="_blank">The Grayzone</a>, an independent news website covering "politics and empire".',
    "Mnar Muhawesh Adley":
      '<a href="https://twitter.com/mnarmuh?lang=en" target="_blank">@MnarMuh</a> is the founder of <a href="https://www.mintpressnews.com/" target="_blank">Mint Press News</a>, an independent news website that covers "imperialism, corporatocracy, and civil liberties".',
    "Noah C Rothman":
      '<a href="https://twitter.com/noahcrothman?lang=en" target="_blank">@NoahCRothman</a> is an MSNBC/NBC contributor, editor of Commentary, and the author of "Unjust: Social Justice and the Unmaking of America".',
    WRPitt:
      '<a href="https://twitter.com/wrpitt?lang=en" target="_blank">@WRPitt</a> is a lead columnist and senior editor for <a href="https://truthout.org/" target="_blank">Truthout</a>, an independent news website focused on revealing systemic injustice.',
  };

  function init() {
    d3.csv(
      "data/jan-21-21-botschedule/PeaceData_TweetSchedule_AlexLacusta.csv",
      function (data) {
        // Scales
        var x = d3
          .scaleBand()
          .range([0, 2 * Math.PI]) // X axis goes from 0 to 2pi = all around the circle. If I stop at 1Pi, it will be around a half circle
          .align(0) // This does nothing
          .domain(
            data.map(function (d) {
              return d["Hour of Day"];
            })
          ); // The domain of the X axis is the times of day
        var y = d3
          .scaleRadial()
          .range([innerRadius, outerRadius]) // Domain will be define later.
          .domain([0, 42]); // Domain of Y is from 0 to the max seen in the data

        // Add circles for amount reference
        var ticks = 2;
        lacusta_sr_svg
          .append("g")
          .selectAll("g")
          .data(y.ticks(ticks))
          .enter()
          .append("circle")
          .attr("stroke", "#000")
          .attr("stroke-width", 1)
          .attr("opacity", "0.4")
          .attr("fill", "transparent")
          .attr("r", y);
        let y_label_container = lacusta_sr_svg
          .append("g")
          .selectAll("g")
          .data(y.ticks(ticks).slice(1))
          .enter()
          .append("g");
        y_label_container
          .append("text")
          .text(function (d) {
            return d + " tweets";
          })
          .style("font-size", "11px")
          .attr("text-anchor", "middle")
          .attr("transform", function (d) {
            return "translate(0," + (-y(d) + 4) + ")";
          })
          .attr("stroke", light)
          .attr("stroke-width", 5);
        y_label_container
          .append("text")
          .text(function (d) {
            return d + " tweets";
          })
          .style("font-size", "11px")
          .attr("text-anchor", "middle")
          .attr("transform", function (d) {
            return "translate(0," + (-y(d) + 4) + ")";
          });

        // Add the bars
        lacusta_sr_svg
          .append("g")
          .selectAll("path")
          .data(data)
          .enter()
          .append("path")
          .attr("fill", lacusta_color)
          .attr("opacity", bar_opacity)
          .attr(
            "d",
            d3
              .arc() // imagine your doing a part of a donut plot
              .innerRadius(innerRadius)
              .outerRadius(function (d) {
                return y(d["Number of Tweets"]);
              })
              .startAngle(function (d) {
                return x(d["Hour of Day"]);
              })
              .endAngle(function (d) {
                return x(d["Hour of Day"]) + x.bandwidth();
              })
              .padAngle(0.01)
              .padRadius(innerRadius)
          );

        // Add the ticks
        var tickLength = 5;
        lacusta_sr_svg
          .append("g")
          .selectAll("g")
          .data(data)
          .enter()
          .append("line")
          .attr("stroke", "#000")
          .attr("stroke-width", 1)
          .attr("x2", 5)
          .attr("transform", function (d) {
            return (
              "rotate(" +
              (((x(d["Hour of Day"]) + x.bandwidth() / 2) * 180) / Math.PI -
                90) +
              ")" +
              "translate(" +
              (innerRadius - tickLength - 2) +
              ",0)"
            );
          });

        // Add the labels
        lacusta_sr_svg
          .append("g")
          .selectAll("g")
          .data(data)
          .enter()
          .append("g")
          .attr("text-anchor", function (d) {
            return (x(d["Hour of Day"]) + x.bandwidth() / 2 + Math.PI) %
              (2 * Math.PI) <
              Math.PI
              ? "end"
              : "start";
          })
          .attr("transform", function (d) {
            return (
              "rotate(" +
              (((x(d["Hour of Day"]) + x.bandwidth() / 2) * 180) / Math.PI -
                90) +
              ")" +
              "translate(" +
              (innerRadius - 30) +
              ",0)"
            );
          })
          .append("text")
          .text(function (d) {
            return d["Hour of Day"];
          })
          .attr("transform", function (d) {
            return (x(d["Hour of Day"]) + x.bandwidth() / 2 + Math.PI) %
              (2 * Math.PI) <
              Math.PI
              ? "rotate(180)"
              : "rotate(0)";
          })
          .style("font-size", "9px")
          .attr("alignment-baseline", "middle")
          .classed("schedule-tick-label", true);
      }
    );

    d3.csv(
      "data/jan-21-21-botschedule/PeaceData_OtherAccountTweetSchedules.csv",
      function (data) {
        // Scales
        var x = d3
          .scaleBand()
          .range([0, 2 * Math.PI]) // X axis goes from 0 to 2pi = all around the circle. If I stop at 1Pi, it will be around a half circle
          .align(0) // This does nothing
          .domain(
            data.map(function (d) {
              return d["Hour of Day"];
            })
          ); // The domain of the X axis is the times of day

        var max_y = d3.max(data.map((d) => d[currentUser]).map(Number));
        var y = d3
          .scaleRadial()
          .range([innerRadius, outerRadius]) // Domain will be define later.
          .domain([0, max_y]); // Domain of Y is from 0 to the max seen in the data

        // Make a container so the text remains behind the bars
        var schedule_circles = norm2_sr_svg
          .append("g")
          .classed("schedule_circles", true);

        // Add circles for amount reference
        var makeCircles = function () {
          var ticks = 2;
          schedule_circles
            .append("g")
            .selectAll("g")
            .data(y.ticks(ticks))
            .enter()
            .append("circle")
            .attr("stroke", "#000")
            .attr("stroke-width", 1)
            .attr("opacity", "0.4")
            .attr("fill", "transparent")
            .classed("schedule_circle", true)
            .attr("r", y);
          let y_label_container = schedule_circles
            .append("g")
            .selectAll("g")
            .data(y.ticks(ticks).slice(1))
            .enter()
            .append("g");
          y_label_container
            .append("text")
            .text(function (d) {
              return d + " tweets";
            })
            .style("font-size", "11px")
            .attr("text-anchor", "middle")
            .attr("transform", function (d) {
              return "translate(0," + (-y(d) + 4) + ")";
            })
            .attr("stroke", light)
            .attr("stroke-width", 5)
            .classed("schedule_label", true);
          y_label_container
            .append("text")
            .text(function (d) {
              return d + " tweets";
            })
            .style("font-size", "11px")
            .attr("text-anchor", "middle")
            .attr("transform", function (d) {
              return "translate(0," + (-y(d) + 4) + ")";
            })
            .classed("schedule_label_bg", true);
        };
        makeCircles();

        // Add the bars
        norm2_sr_svg
          .append("g")
          .selectAll("path")
          .data(data)
          .enter()
          .append("path")
          .classed("schedule_bar", true)
          .attr("fill", norm_color)
          .attr("opacity", bar_opacity)
          .attr(
            "d",
            d3
              .arc() // imagine your doing a part of a donut plot
              .innerRadius(innerRadius)
              .outerRadius(function (d) {
                let num = d[currentUser];
                num = num == 0 ? 0.1 : num; // Hacky way to make the transitions work better from 0
                return y(num);
              })
              .startAngle(function (d) {
                return x(d["Hour of Day"]);
              })
              .endAngle(function (d) {
                return x(d["Hour of Day"]) + x.bandwidth();
              })
              .padAngle(0.01)
              .padRadius(innerRadius)
          );

        function updateSchedule(selectedGroup) {
          let duration = 800;

          currentUser = selectedGroup;

          // Change chart description for selected user
          d3.select("#schedule_select_desc").html(
            user_descriptions[currentUser]
          );

          // Re-scale y
          max_y = d3.max(data.map((d) => d[currentUser]).map(Number));
          y = d3
            .scaleRadial()
            .range([innerRadius, outerRadius]) // Domain will be define later.
            .domain([0, max_y]); // Domain of Y is from 0 to the max seen in the data

          // Resize bars for new data
          norm2_sr_svg
            .selectAll(".schedule_bar")
            .data(data)
            .transition()
            .duration(duration)
            .attr(
              "d",
              d3
                .arc() // imagine your doing a part of a donut plot
                .innerRadius(innerRadius)
                .outerRadius(function (d) {
                  let num = parseInt(d[currentUser]);
                  num = num == 0 ? 0.1 : num;
                  return y(num.toString());
                })
                .startAngle(function (d) {
                  return x(d["Hour of Day"]);
                })
                .endAngle(function (d) {
                  return x(d["Hour of Day"]) + x.bandwidth();
                })
                .padAngle(0.01)
                .padRadius(innerRadius)
            );

          // Add circles for amount reference
          norm2_sr_svg.selectAll(".schedule_circle").remove();
          norm2_sr_svg.selectAll(".schedule_label").remove();
          norm2_sr_svg.selectAll(".schedule_label_bg").remove();
          makeCircles();
        }

        // When the button is changed, run the updateChart function
        d3.select("#schedule_select").on("change", function (d) {
          // recover the option that has been chosen
          var selectedOption = d3.select(this).property("value");
          // run the updateChart function with this selected option
          updateSchedule(selectedOption);
        });

        // Add the ticks
        var tickLength = 5;
        norm2_sr_svg
          .append("g")
          .selectAll("g")
          .data(data)
          .enter()
          .append("line")
          .attr("stroke", "#000")
          .attr("stroke-width", 1)
          .attr("x2", 5)
          .attr("transform", function (d) {
            return (
              "rotate(" +
              (((x(d["Hour of Day"]) + x.bandwidth() / 2) * 180) / Math.PI -
                90) +
              ")" +
              "translate(" +
              (innerRadius - tickLength - 2) +
              ",0)"
            );
          });

        // Add the labels
        norm2_sr_svg
          .append("g")
          .selectAll("g")
          .data(data)
          .enter()
          .append("g")
          .attr("text-anchor", function (d) {
            return (x(d["Hour of Day"]) + x.bandwidth() / 2 + Math.PI) %
              (2 * Math.PI) <
              Math.PI
              ? "end"
              : "start";
          })
          .attr("transform", function (d) {
            return (
              "rotate(" +
              (((x(d["Hour of Day"]) + x.bandwidth() / 2) * 180) / Math.PI -
                90) +
              ")" +
              "translate(" +
              (innerRadius - 30) +
              ",0)"
            );
          })
          .append("text")
          .text(function (d) {
            return d["Hour of Day"];
          })
          .attr("transform", function (d) {
            return (x(d["Hour of Day"]) + x.bandwidth() / 2 + Math.PI) %
              (2 * Math.PI) <
              Math.PI
              ? "rotate(180)"
              : "rotate(0)";
          })
          .style("font-size", "9px")
          .attr("alignment-baseline", "middle")
          .classed("schedule-tick-label", true);
      }
    );
  }

  onMount(() => {
    lacusta_sr_svg = d3
      .select("#lacusta_schedule_radial svg")
      .attr("width", width + rad_margin.left + rad_margin.right)
      .attr("height", height + rad_margin.top + rad_margin.bottom)
      .append("g")
      .attr(
        "transform",
        "translate(" +
          (width / 2 + rad_margin.left) +
          "," +
          (height / 2 + rad_margin.top) +
          ")"
      );
    norm2_sr_svg = d3
      .select("#norm2_schedule_radial svg")
      .attr("width", width + rad_margin.left + rad_margin.right)
      .attr("height", height + rad_margin.top + rad_margin.bottom)
      .append("g")
      .attr(
        "transform",
        "translate(" +
          (width / 2 + rad_margin.left) +
          "," +
          (height / 2 + rad_margin.top) +
          ")"
      );

    d3.select("#schedule_select")
      .selectAll("schedule_options")
      .data(users)
      .enter()
      .append("option")
      .text(function (d) {
        return d;
      })
      .attr("value", function (d) {
        return d;
      });

    d3.select("#schedule_select_desc").html(user_descriptions[currentUser]);

    init();
  });
</script>

<div id="schedules">
  <div class="schedule">
    <p class="schedule_title">Total Tweets per Hour of</p>
    <p class="schedule_name not_select">Alex Lacusta (PeaceData Editor)</p>
    <div class="fixed_height">
      <p class="schedule_desc">
        @AlexLacusta was the "editor" of PeaceData that tweeted the most. The
        account tweeted 227 times before being taken down.
      </p>
    </div>
    <div id="lacusta_schedule_radial">
      <svg />
    </div>
    <p class="schedule_desc">
      (Total # of tweets per hour between the dates of July 8, 2020 and Aug. 19,
      2020.)
    </p>
  </div>
  <div class="schedule">
    <p class="schedule_title">Total Tweets per Hour of</p>
    <select id="schedule_select" class="schedule_name" />
    <div class="fixed_height">
      <p class="schedule_desc" id="schedule_select_desc" />
    </div>
    <div id="norm2_schedule_radial">
      <svg />
    </div>
    <p class="schedule_desc">
      (Total # of tweets per hour between the dates of July 8, 2020 and Aug. 19,
      2020.)
    </p>
  </div>
</div>

<style>
  #schedules {
    width: 100%;
    height: auto;
    display: flex;
    justify-content: center;
    margin: 50px 0;
  }

  .schedule {
    display: flex;
    text-align: center;
    align-items: center;
    flex-direction: column;
  }

  .schedule_title,
  .schedule_name {
    font-size: 0.8em;
    margin: 0;
  }

  .schedule_name {
    font-weight: 500;
    margin-top: 10px;
  }

  .not_select {
    margin-top: 20px;
    margin-bottom: 5px;
  }

  .schedule_desc {
    font-size: 0.6em;
    max-width: 250px;
    margin-top: 20px;
    margin-bottom: 0;
  }

  .schedule .fixed_height {
    height: 70px;
    display: flex;
    align-items: center;
  }

  @media screen and (max-width: 600px) {
    #schedules {
      flex-direction: column;
    }

    .schedule {
      margin-top: 40px;
    }

    .schedule_title {
      font-weight: 600;
    }

    .not_select {
      margin-top: 10px;
      margin-bottom: 0;
    }
  }
</style>
