---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
---
<div class="container">
  <div class="content">
    <div id="statement">
      <div id="left">
        <h3>About Me</h3>
        <p>I am a software engineer living in Melbourne, Australia. Also, I got my
        PhD from the University of Melbourne. During my PhD study, I worked with Professor
          <a href="https://scholar.google.com/citations?user=3kVu88AAAAAJ&amp;hl=en&amp;oi=ao">Lars Kulik</a> and Professor
          <a href="https://scholar.google.com/citations?user=UrYhkwQAAAAJ&amp;hl=en">Rao Kotagiri</a>
          in sptio-temporal data mining, such as traffic data analytics and predictions,
          geospatial map applications, geospatial data visualization, and etc.
        </p>
        <p>
        My current interests are:
          <ul>
            <li>Distributed systems;</li>
            <li>Real-time stream data processing;</li>
            <li>Docker, kubernetes, and container related techniques;</li>
            <li>Scalable and maintainable data systems;</li>
            <li>Recommender systems</li>
          </ul>
        </p>
    		<p></p>
    		<p>
          Email: <img src="/assets/img/my-email.png" style="display:inline-block;"><br>
          Github: <a href="https://github.com/HengfengLi">github.com/HengfengLi</a>
  	    </p>
        <p>
          A pdf version of my CV can be downloaded <a href="/assets/docs/hengfengli-cv-no-mobile.pdf">here.</a>
        </p>
      </div>

      <div id="right">
        <img id="myphoto" src="http://www.gravatar.com/avatar/791887831fad3e96fcc979748a5102eb?s=400">
      </div>
    </div>

    <div id="research">
      <p></p>
      <h3>Education</h3>
      <div class="edu">
        <div class="edu-icon"><img src="/assets/img/edu-icon.png" alt=""></div>
        <div class="edu-item">
          <span class="edu-title">Ph.D. in Computer Science,</span>
          <span class="edu-venue">The University of Melbourne, Australia</span>
          <span>(thesis approved on Nov 2016)</span>
        </div>
      </div>

      <div class="edu">
        <div class="edu-icon"><img src="/assets/img/edu-icon.png" alt=""></div>
        <div class="edu-item">
          <span class="edu-title">M.E. in Distributed Computing,</span>
          <span class="edu-venue">The University of Melbourne, Australia</span>
          <span>2012</span>
        </div>
      </div>

      <div class="edu">
        <div class="edu-icon"><img src="/assets/img/edu-icon.png" alt=""></div>
        <div class="edu-item">
          <span class="edu-title">B.E. in Software Engineering,</span>
          <span class="edu-venue">Beihai College of Beihang University, China</span>
          <span>2010</span>
        </div>
      </div>
    	<p></p>

      <p></p>
      <h3>Publications</h3>
      <!-- Chicago ref. style -->
      <ul>
        <li>
          <b>Li, Hengfeng</b>, Lars Kulik, and Kotagiri Ramamohanarao. “Automatic Generation and Validation of Road Maps From GPS Trajectory Data Sets.” Submitted to the 25th ACM International Conference on Information and Knowledge Management (<b>CIKM 2016</b>)
          (pp. 1523-1532). ACM. [<a href="https://people.eng.unimelb.edu.au/henli/docs/cikm16.pdf">PDF</a>]
        </li>
        <li>
          <b>Li, Hengfeng</b>, Lars Kulik, and Kotagiri Ramamohanarao. “Robust inferences of travel paths from GPS trajectories.” International Journal of Geographical Information Science (<b>IJGIS</b>) 29, no. 12 (2015): 2194-2222. [<a href="https://people.eng.unimelb.edu.au/henli/docs/ijgis15-submitted.pdf">PDF</a>]
          [<a href="http://people.eng.unimelb.edu.au/henli/projects/robust-inferences-of-travel-paths/">Code</a>]
        </li>
        <li>
          <b>Li, Hengfeng</b>, Lars Kulik, and Kotagiri Ramamohanarao. “Spatio-temporal trajectory simplification for inferring travel paths.” In Proceedings of the 22nd ACM SIGSPATIAL International Conference on Advances in Geographic Information Systems (
          <b>SIGSPATIAL 2014</b>), pp. 63-72. ACM, 2014. (Acceptance rate: 21.2%)
          [<a href="https://people.eng.unimelb.edu.au/henli/docs/sigspatial14-camera-ready.pdf">PDF</a>]
          [<a href="http://people.eng.unimelb.edu.au/henli/projects/map-matching/">Data</a>]
        </li>
      </ul>
      <p></p>
    </div>

    <div id="experience">
  		<h3>Professional Experience</h3>
      <div class="work-exp">
        <div class="work-icon"><img src="/assets/img/work-icon.png" alt=""></div>
        <div class="work-item">
          <div class="work-venue">Ladoo Pty. Ltd.</div>
          <div class="work-title">Software Developer</div>
          <div class="work-duration">Oct 2016 — Present</div>

          <p>
          I mainly work on developing and maintaining a social media archiving platform, Brolly (<a href="https://brolly.com.au/">https://brolly.com.au/</a>).
          </p>

          <div>
            Achievements:
            <ul>
              <li>Architected, built, and tested the new data capture pipeline based on an event-stream architecture with techniques like AWS Kinesis, DynamoDB, Celery, RabbitMQ.</li>
              <li>Implemented new data crawlers for Facebook, Twitter, Instagram, and YouTube.</li>
              <li>Worked on containerizing the monopolistic application into microservices via Docker and AWS ECS.</li>
              <li>Worked on the export service to query and process data from ElasticSearch and generate an offline HTML app.</li>
              <li>Created and maintained a suite of automated tests, including unit tests and integration tests.</li>
            </ul>
          </div>
          <div>Tech Used: Python, Celery, Docker, PHP, Symfony, MySQL, AWS (ECS, Kinesis, Dynamodb, SQS, ElastiCache, Elastic Beanstalk, CodePipeline, CodeBuild, etc), ElasticSearch, RabbitMQ, JavaScript, Backbone.js, Flask, Jenkins</div>
        </div>
      </div>

      <div class="work-exp">
        <div class="work-icon"><img src="/assets/img/work-icon.png" alt=""></div>
        <div class="work-item">
          <div class="work-venue">The University of Melbourne</div>
          <div class="work-title">Tutor/Head Tutor</div>
          <div class="work-duration">Mar 2013 — Jul 2016</div>

          <div>
            Achievements:
            <ul>
              <li>Head Tutor for COMP90020 Distributed Algorithms (2016 Semester 1). </li>
              <li>Tutor for COMP90038 Algorithms and Complexity (2016 Semester 1). </li>
              <li>Tutor for COMP90018 Mobile Computing Systems Programming (2015 Semester 2).</li>
              <li>Lab demonstrator for GEOM90042 Spatial Information Programming (2015 Semester 1). </li>
              <li>Tutor for COMP90017 Sensor Networks and Applications (2013 Semester 2). </li>
              <li>Lab demonstrator for GEOM90042 Spatial Information Programming (2013 Semester 1). </li>
            </ul>
          </div>
          <div>Tech Used: Python, Java, Swift, iOS programming</div>
        </div>
      </div>

      <div class="work-exp">
        <div class="work-icon"><img src="/assets/img/work-icon.png" alt=""></div>
        <div class="work-item">
          <div class="work-venue">The University of Melbourne</div>
          <div class="work-title">PhD Candidate</div>
          <div class="work-duration">Jul 2012 — Jun 2016</div>

          <div class="work-research">Research Project: Road Maps Generation from Imprecise GPS Trajectory Data <a href="https://people.eng.unimelb.edu.au/henli/docs/cis-poster-2016.pdf">[Poster'16]</a></div>
          <div>
            Achievements:
            <ul>
              <li>Processed and cleaned a large amount of GPS trajectory data. </li>
              <li>Developed an algorithm to infer road maps from GPS trajectory data. </li>
              <li>Designed and implemented evaluation experiments. </li>
            </ul>
          </div>
          <div>Tech Used: Python, Numpy, Matplotlib, scikit-learn, Scipy</div>

          <div class="work-research">Research Project: Mining Large-scale GPS Trajectory Data Sets <a href="https://people.eng.unimelb.edu.au/henli/docs/cis-poster-2014.pdf">[Poster'14]</a><a href="https://people.eng.unimelb.edu.au/henli/docs/cis-poster-2015.pdf">[Poster'15]</a></div>
          <div>
            Achievements:
            <ul>
              <li>Processed and analyzed map data from OpenStreetMap. </li>
              <li>Designed and developed inference algorithms to reconstruct travel paths from imprecise trajectory data. </li>
              <li>Measured the performance metrics and evaluated the accuracy. </li>
            </ul>
          </div>
          <div>Tech Used: Java, Python, Numpy, Matplotlib</div>

          <div class="work-research">Distributed Microscopic Traffic Simulator <a href="https://people.eng.unimelb.edu.au/henli/docs/cis-poster-2013.pdf">[Poster'13]</a><a href="https://vimeo.com/183649680">[Demo A]</a><a href="https://vimeo.com/183651859">[Demo B]</a></div>
          <div>
            Achievements:
            <ul>
              <li>Refactored the core part of microscopic vehicle simulation. </li>
              <li>Developed a web application for visualizing the movement of vehicles. </li>
              <li>Processed and analyzed the complex traffic data generated by the simulator. </li>
            </ul>
          </div>
          <div>Tech Used: JavaScript, Processing.js, jQuery, OpenStreetMap, Java, Apache Thrift, PHP</div>

        </div>
      </div>

      <div class="work-exp">
        <div class="work-icon"><img src="/assets/img/work-icon.png" alt=""></div>
        <div class="work-item">
          <div class="work-venue">Baidu Inc. </div>
          <div class="work-title">Front-end Engineer (Intern)</div>
          <div class="work-duration">Dec 2011 — Feb 2012</div>

          <div>
            Achievements:
            <ul>
              <li>Implemented and maintained web pages for important public activities and events. </li>
              <li>Developed an internal tool to manage the usage of multiple development servers. </li>
              <li>Added new features to the internal project management system, such as asynchronous data update and autocompletion. </li>
            </ul>
          </div>
          <div>Tech Used: HTML, CSS, JavaScript, PHP, Python, Shell script, jQuery, Ajax</div>
        </div>
      </div>

    	<p></p>
      <h3>Awards and Scholarships</h3>
      <ul>
        <li> Excellence in Tutoring Award in the Department of Computing and Information Systems in 2016 SEM1.</li>
        <li> Best poster award on CIS Doctoral Colloquium in the University of Melbourne in
          <a href="https://people.eng.unimelb.edu.au/henli/docs/cis-poster-2014.pdf">2014</a>,
          <a href="https://people.eng.unimelb.edu.au/henli/docs/cis-poster-2015.pdf">2015</a>,
          <a href="https://people.eng.unimelb.edu.au/henli/docs/cis-poster-2016.pdf">2016</a>.
        </li>
        <li> Student volunteer for ACM SIGMOD 2015 (Melbourne, Australia).</li>
        <!-- <li> Participant of Ford SYNC AppLink Hackathon 2013 (Melbourne). </li> -->
        <li> Recipient of the scholarships MIRS and MIFRS by the University of Melbourne, 2012.</li>
        <li> One of Part 2 winners in IBM Master the Mainframe Contest Australia 2012. </li>
        <li> Recipient of the Dean's Honor Award (top 5% student) in the School of Engineering in 2011.</li>
        <li> The 2nd National Award on CUMCM (China Undergraduate Mathematical Contest in Modeling) in 2008. </li>
      </ul>
      <p></p>
    </div>

    <div id="program">
      <p>
      </p>
      <h3>Projects</h3>
      <ul>
        <li><a href="https://people.eng.unimelb.edu.au/henli/programs/duality-demo">points and lines duality demo</a> : An interactive demo to illustrate duality of points and lines.
        </li>
        <li><a href="https://github.com/HengfengLi/pymapplot">pymapplot</a> : Overlays on map tiles in Python.
        </li>
        <li><a href="https://github.com/HengfengLi/osm-parser">osm-parser</a> : A parser for osm files that contains the map data from OpenStreetMap..
        </li>
        <li><a href="https://github.com/HengfengLi/spatial-knn-implementations">knn-grid-impl</a> : A grid indexing implementation of KNN on nodes and edges of a graph.
        </li>
        <li><a href="https://github.com/HengfengLi/leetcode-oj">leetcode-oj</a> : My solutions for Leetcode Online Judge in Python.
        </li>
        <li><a href="https://github.com/HengfengLi/SICP-Solutions">sicp-solutions</a> : My solutions for exercises in Structure and Interpretation of Computer Programs (SICP).
        </li>
        <li><a href="https://github.com/HengfengLi/text-calendar-generator">text-calendar</a> : An interesting text calendar generator.
        </li>
        <li><a href="https://github.com/HengfengLi/Spelling-Correction">spelling-correction</a> : Some algorithms for spelling correction.
        </li>
        <li><a href="https://github.com/HengfengLi/Higher-IBM-Models">higher-ibm-models</a> : Higher IBM models for statistical machine translation.
        </li>
      </ul>
      <p></p>
    </div>
  </div>
</div>
