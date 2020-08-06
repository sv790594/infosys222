# <i class="fas fa-database"></i> Case study
### Hospital management system
[<i class="fab fa-creative-commons"></i>](https://creativecommons.org/licenses/by/4.0/) [Johnny Chan](mailto:jh.chan@auckland.ac.nz) | <i class="fas fa-calendar"></i> 2020-08-10



## Background
- Auckland City Hospital (ACH) is New Zealand's largest and oldest public hospital. It is run by the Auckland District Health Board (ADHB), located in the suburb of Grafton

- To manage around 80,000 patients annually, the hospital management system plays a critical role on a daily basis



## Specification
- The following specification captures only part of the hospital management system. The case is aimed for learning and practicing data modelling; it has no affiliation with ACH or ADHB


## Person
- ACH depends on four major groups of people for its continued success: employee, physician, patient and volunteer. A person in the hospital community could belong to none, one or more of these groups

- The four groups of people share many common characteristics such as an unique identifier, last name, first name, address, birth date, phone, and email. There are characteristics that apply to only one or two of these groups. For example:
  - a hire date is recorded for employee only
  - interests and skills are recorded for volunteer only
  - a pager number and a MC number are recorded for physician only (the MC number is registered with Medicines Control from the Ministry of Health for physician to prescribe controlled drugs)
  - a first contact date is recorded for patient only
  - a speciality (e.g. pediatrics) is recorded for physician and nurse only


## Patient
- In addition to the characteristics already mentioned, the hospital records a number of other characteristics about patient:
  - emergency contact information (last name, first name, relationship to patient, address, and phone)
  - insurance information (company name, policy number, group number, and phone)
  - insurance subscriber information in case the patient is not the insurance subscriber (last name, first name, relationship to patient, address, and phone)
  - contact information for the patient's primary care physician or other physician who referred the patient to the hospital
- Each patient has one and only one physician responsible for them


## Resident and Outpatient
- The primary patient segments are resident and outpatient

- Outpatient may come in for many reasons, including routine examination, ambulatory / outpatient surgery, diagnostic service, or emergency room care

- Each outpatient is scheduled for zero or more visits. A visit has several attributes: an unique identifier, date, and time; it could not exist without an outpatient

- An outpatient, for example, in the emergency room, could be subsequently admitted to the hospital and becomes a resident

- Each resident has a date admitted and a date discharged


## Volunteer
- Volunteer works in many areas of the hospital based on their interests and skills

- The volunteer's time of service (start date and end date), work unit and supervisor information are recorded. Each volunteer is supervised by an employee or physician

- The system also tracks volunteer's number of hours worked and recognises outstanding volunteer at an annual award ceremony


## Employee: Nurse
- Employee falls into three categories: nurse, technician and staff

- Each nurse has a certificate / degree indicating their qualification as an enrolled nurse or registered nurse (enrolled nurse works under the direction of registered nurse in ACH). Some nurse may hold certification in special field like dialysis, pediatrics, anesthesia, critical care, pain management etc

- Most nurses are assigned to only one care centre at a time, although over time, they may be working in more than one care centre. Some nurses are floaters who are not assigned to a specific care centre but instead work wherever they are needed

- One registered nurse is appointed as nurse in charge in a care centre


## Employee: Technician
- Specific job-related skills are recorded for the technician. A cardiovascular technician may be skilled in specific equipment, such as setting up and getting reading from a Holter monitor, a portable device that monitors a patient's EKG for a period of 24 to 48 hours during routine activity

- Medical laboratory technician is abled to set up, operate, and control equipment, perform a variety of tests, analyse the test data, and summarise test results for physician who uses them to diagnose and treat patient

- Emergency room technician could perform CPR or setup an IV

- Dialysis technician, who may be skilled in different types of dialysis (e.g. pediatric dialysis, outpatient dialysis), needs a variety of skills related to setting up treatment, assessing the patient during dialysis, and assessing and troubleshooting equipment problems during dialysis

- Each technician is assigned to a work unit (e.g. a care centre, the central medical laboratory, radiology etc)


## Employee: Staff
- Staff member has a job classification, such as secretary, administrative assistant, admitting specialist, collection specialist, and so on

- Like the technician, each staff member is assigned to a work unit (e.g. a care centre, the central medical laboratory, radiology etc)


## Work Unit
- Work unit such as a care centre has a name and location. The location denotes the facility (e.g. main building) and floor (e.g. 3 West, 2 South)

- A care centre often has one or more beds assigned to it, but some may not have any assigned beds. The only attribute of bed is an unique identifier

- Each resident must be assigned to a bed



# THE END
<canvas width=400 height=400 class="anything">
<!--
{
  "initialize": "function(container) {
	var width = container.width,
	    height = container.height;
	var projection = d3.geo.orthographic()
	    .translate([width / 2, height / 2])
	    .scale(width / 2 - 20)
	    .clipAngle(90)
	    .precision(0.6);

	var c = container.getContext('2d');

	var path = d3.geo.path()
	    .projection(projection)
	    .context(c);

	var title = container.parentElement.querySelector('.country');
	queue()
	    .defer(d3.json, '../asset/globe/world-110m.json')
	    .defer(d3.tsv, '../asset/globe/world-country-names.tsv')
	    .await(ready);

	function ready(error, world, names) {
	  if (error) throw error;

	  var globe = {type: 'Sphere'},
	      land = topojson.feature(world, world.objects.land),
	      countries = topojson.feature(world, world.objects.countries).features,
	      borders = topojson.mesh(world, world.objects.countries, function(a, b) { return a !== b; }),
	      i = -1,
	      n = countries.length;

	  countries = countries.filter(function(d) {
	    return names.some(function(n) {
	      if (d.id == n.id) return d.name = n.name;
	    });
	  }).sort(function(a, b) {
	    return a.name.localeCompare(b.name);
	  });

	  (function transition() {
	    d3.transition()
	        .duration(1250)
	        .each('start', function() {
			while ( !countries[i = (i + 1) % n] ) {};			
			title.innerHTML = (countries[i].name);
	        })
	        .tween('rotate', function() {
	          var p = d3.geo.centroid(countries[i]),
	              r = d3.interpolate(projection.rotate(), [-p[0], -p[1]]);
	          return function(t) {
	            projection.rotate(r(t));
	            c.clearRect(0, 0, width, height);
	            c.fillStyle = '#fff', c.lineWidth = 2, c.beginPath(), path(globe), c.fill();
	            c.fillStyle = '#42affa', c.beginPath(), path(land), c.fill();
	            c.fillStyle = '#f00', c.beginPath(), path(countries[i]), c.fill();
	            c.strokeStyle = '#ccc', c.lineWidth = .5, c.beginPath(), path(borders), c.stroke();
	            c.strokeStyle = '#ccc', c.lineWidth = 2, c.beginPath(), path(globe), c.stroke();
	          };
	        })
	      .transition()
	        .each('end', transition);
	  })();
	}

	d3.select(self.frameElement).style('height', height + 'px');

    }"
}
-->
</canvas>

Database is awesome in <span class="country">everywhere</span>!

[<i class="fas fa-print"></i>](?print-pdf#)
