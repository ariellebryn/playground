<script>
  import PostParagraph from "../../components/PostParagraph.svelte";

  import PDBotSchedule from "./components/PDBotSchedule.svelte";
</script>

<PostParagraph>
  On September 1, 2020, Twitter announced
  <a href="https://twitter.com/TwitterSafety/status/1300848632120242181"
    >the suspension of five accounts</a
  >
  controlled by "Russian state actors".
</PostParagraph>
<PostParagraph>
  The accounts purported to be associated with a website called PeaceData, which
  published a range of content about global political issues. While the content
  published on the site seems to have been authored by real writers, the Twitter
  accounts were found to be fake—bots or Russian <a
    href="https://www.nytimes.com/2020/09/01/technology/facebook-russia-disinformation-election.html"
    target="_blank">Internet Research Agency</a
  > workers.
</PostParagraph>
<PostParagraph>
  Twitter was alerted to the accounts by the FBI, but there <i>were</i> signs that
  these accounts were fake that Twitter could have found if they had known to look.
  Fake photos, for instance, as well as another element of fake social media accounts
  that I wanted to see for myself: their posting schedule.
</PostParagraph>
<div class="jan2121">
  <a
    href="https://twitter.com/newsjager/status/1301052315093393413"
    target="_blank">
    <!-- svelte-ignore a11y-missing-attribute -->
    <img src="images/jan-21-21-botschedule/alexlacusta.jpg" />
  </a>
  <p class="caption">
    A photo of the Alex Lacusta Twitter account before it was taken down.
    (Source: Twitter)
  </p>
</div>
<PostParagraph>
  If the accounts were run by people working for the IRA, then tweeting is a
  part of their job. Bots and fake accounts often have a schedule when they
  post, and that schedule often aligns with what we might think of as "shift"
  hours. I compared the tweeting schedules from the most prolific fake account,
  Alex Lacusta, and other accounts held by people with statuses similar to the
  one Lacusta claimed to have: editor, writer, radical.
</PostParagraph>
<PostParagraph
  >After gathering all the tweets from each account between July 8, 2020 and
  August 19, 2020, I bucketed the tweets by the hour of day they were posted and
  summed the number of tweets in each hour across the days. Select different
  accounts on the right to see how the schedules compare:
</PostParagraph>

<PDBotSchedule />

<PostParagraph>
  <b>Alex Lacusta</b> tweeted almost entirely between the hours of noon and
  seven p.m. between July 8 and August 19, 2020.
  <b
    ><i>He has very few tweets outside of this seven hour period each day.</i
    ></b
  >
</PostParagraph>
<PostParagraph>
  Now look at the total number of tweets by hour for the <b>real users</b> on
  the right.
  <b><i>Each of them tweet much more equally through their waking hours.</i></b>
</PostParagraph>
<PostParagraph>
  This isn't a scientific study. The sample is small, and an editor <i>could</i>
  only tweet as a part of their job for their publication, causing a reasonable shift-like
  schedule for their tweets. Still, knowing that Alex Lacusta was never real to begin
  with, it's interesting to see how the schedule lines up with what we know about
  fake accounts: that someone at the IRA sat down every day for approximately seven
  hours and banged out tweet after tweet, working to make this account look as real
  as possible.
</PostParagraph>

<style>
  .jan2121 img {
    width: 100%;
  }
  .caption {
    font-size: 0.7em;
    text-align: center;
    margin-top: 3px;
    opacity: 0.9;
  }
</style>
