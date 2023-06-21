# JOB PORTAL APPLICATION USING REACT, SPRINGBOOT AND SQL
## AIM
To create a job portal application using react, springboot and sql.
## ALGORITHM
1. Configure a new project with the required dependencies and files.
2. Initialise the same in IntelliJ and include file Patient.java which contains all the variables like Applicant's ame,phone number,Email and their respective constructor,getters and setters.
3. Include Components and service files which configures and works for commands like GET,POST and DELETE.
4. Run the file by connecting to an database by creating the same in psql.
5. Start an react project named Job-portal-frontend
6. Include necessary functions in App.js
7. Include the necessary .css files for all the three methods.
8. Define separate .js files for viewing the list of Applicants,adding Applicants and deleting an Applicant.
9. Connect with the backend server and view the output user interface in the browser.
## PROGRAM
### SpringBoot Program
### Job.java
```
package com.saveetha.jobApplicant.job;
@Entity
@Table
public class Job {
    @Id
    @SequenceGenerator(
            name="jobSequence",
            sequenceName = "jobSequence",
            allocationSize = 1
    )
    @GeneratedValue(
            strategy = GenerationType.SEQUENCE,
            generator = "jobSequence"
    )
    private Long jobApplicantID;
    private String jobApplicantName;
    private Long jobApplicantPhNumber;
    private String jobApplicantEmail;
    public Job() {
    }
    public Job(String jobApplicantName, Long jobApplicantPhNumber,String jobApplicantEmail) {
        this.jobApplicantName = jobApplicantName;
        this.jobApplicantPhNumber = jobApplicantPhNumber;
        this.jobApplicantEmail = jobApplicantEmail;
    }
    public Long getjobApplicantID() {
        return jobApplicantID;
    }
    public String getjobApplicantName() {
        return jobApplicantName;
    }
    public Long getjobApplicantPhNumber() {
        return jobApplicantPhNumber;
    }
    public String getjobApplicantEmail() {
        return jobApplicantEmail;
    }

    public void setjobApplicantID(Long jobApplicantID) {
        this.jobApplicantID = jobApplicantID;
    }
    public void setjobApplicantName(String jobApplicantName) {
        this.jobApplicantName = jobApplicantName;
    }
    public void setjobApplicantPhNumber(Long jobApplicantPhNumber) {
        this.jobApplicantPhNumber = jobApplicantPhNumber;
    }
    public void setjobApplicantEmail(String jobApplicantEmail) {
        this.jobApplicantEmail = jobApplicantEmail;
    }
@Override
    public String toString() {
        return "jobApplicant{" +
                "jobApplicantId=" + jobApplicantID +
                ", jobApplicantName='" + jobApplicantName + '\'' +
                ", jobApplicantPhNumber=" + jobApplicantPhNumbere +
                ", jobApplicantEmail=" + jobApplicantEmail + '\'' +
                '}';
    }
}
```
### JobApplicantController.java
```
package com.saveetha.jobApplicant.job;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import java.util.List;
@RestController
@RequestMapping(path="/api/v1/job")
public class JobApplicantController {
    private final JobService jobService;
    @Autowired
    public JobApplicantController(JobService jobService) {
        this.jobService = jobService;
    }
    @GetMapping("/")
    public List<Job> getJobDetails(){
        return jobService.displayJobtDetails();
    }
    @PostMapping("/")
    public void postjob(@RequestBody Job job){
        jobService.registerNewJobApplicant(job);
    }
    @DeleteMapping(path={"/{id}"})
    public void deleteJobApplicant(@PathVariable("id") Long jobApplicantId)
    {
        jobService.removeJobApplicant(jobApplicantId);
    }
}
```
### React Program
### App.js
```
import React from "react"
import './App.css';
import { BrowserRouter as Router, Route, Routes, Link } from "react-router-dom";
import JobRegistrationComponent from './components/JobRegistrationComponent/JobRegistrationComponent';
import HeaderComponent from "./components/HeaderComponent/HeaderComponent";
import JobDirectoryComponent from "./components/JobDirectoryComponent/JobDirectoryComponent";
import JobDeletionComponent from "./components/JobDeletionComponent/JobDeletionComponent";

function App() {
  return (
    <Router>
            <div className="container">
              <HeaderComponent/>
              
            <nav className="nav-menu">
                <Link to="/" >JApplicant Details</Link>
                <Link to="/admin/add" >Add JApplicant</Link>
                <Link to="/admin/delete" >Delete Applicant</Link>
            </nav>
           <Routes>
                 <Route exact path='/' element={<JobDirectoryComponent/>}></Route>
                 <Route path='/admin/add' element={<JobRegistrationComponent/>}></Route>
                 <Route path='/admin/delete' element={<JobDeletionComponent/>}></Route>
          </Routes>
          </div>
       </Router>
  );
}

export default App;
```
### JobApplicantDirectoryComponent.js
```
import React, { useEffect, useState } from 'react';
import './JobDirectoryComponent.css'; 
function JobDirectoryComponent() {
  const [Job, setJob] = useState([]);
  useEffect(() => {
    fetchJob();
  }, []);
  const fetchJob = async () => {
    try {
      const response = await fetch('http://localhost:8080/api/v1/Job/');
      const data = await response.json();
      setJob(data);
    } catch (error) {
      console.error('Error:', error);
    }
  };
  return (
    <div className="Job-list">
      {Jobs.map((Job) => (
        <div className="Job-card" key={Job.jobApplicantID}>
          <p>JobApplicant ID: {job.jobApplicantID}</p>
          <p>JobApplicant Name: {job.jobApplicantName}</p>
          <p>JobApplicant PhNumber: {job.jobApplicantPhNumber}</p>
          <p>JobApplicant Email: {job.jobApplicantEmail}</p>
        </div>
      ))}
    </div>
  );
};

export defaul JobDirectoryComponent;
```
### JobRegistrationComponent.js
```
import React, { useState } from 'react';
import './JobRegistrationComponent.css'

function JobRegistrationComponent() {
  const [job, setJob] = useState({
    jobApplicantName: '',
    jobApplicantPhNumber: '',
    jobApplicantEmail: ''
  });

  const handleChange = (event) => {
    const { name, value } = event.target;
    setJob({ ...job, [name]: value });
  };

  const handleSubmit = async(event) => {
    event.preventDefault();
    await fetch('http://localhost:8080/api/v1/job/',{
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(job)
    })
        .then((response) => {
            if (response.status === 500)
            {
                alert(`Error!`)
            }
            else{
                alert(`Data of ${job.jobApplicantName} is added successfully`)
                window.location.href = '/'
            }
        })
  };
  return (
    <form onSubmit={handleSubmit}>
      <label>
        Applicant Name:
        <input
          type="text"
          name="applicantName"
          value={job.jobApplicantName}
          onChange={handleChange}
          required
        />
      </label>
      <label>
        Applicant Ph Number:
        <input
          type="number"
          name="applicantNumber"
          value={job.jobApplicantPhNumber}
          onChange={handleChange}
          required
        />
      </label>
      <label>
        Applicant E-mail
        <input
          type="email"
          name="applicantEmail"
          value={job.jobAppicantEmail}
          onChange={handleChange}
          required
        />
      </label>
      
      <button type="submit">Submit</button>
    </form>
  );
};
export default JobRegistrationComponent;
```
### JobDeletionComponent.js
```
import React, { useState } from 'react';
import './JobDeletionComponent.css'

function JobDeletionComponent() {
  const [jobID, setJobID] = useState('');

  const handleChange = (event) => {
    setJobID(event.target.value);
  };

  const handleSubmit = async(event) => {
    event.preventDefault();
    await fetch(`http://localhost:8080/api/v1/job/${jobID}`,{
      method: 'DELETE'
    })
    .then((response) => {
            if (response.status === 500)
            {
                alert(`Error!`)
            }
            else{
                alert(`Data deleted successfully`)
                window.location.href = '/'
            }
        })
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Job Applicant ID:
        <input
          type="number"
          name="jobID"
          value={jobApplicantID}
          onChange={handleChange}
          required
        />
      </label>
      <button type="submit">Submit</button>
    </form>
  );
};

export default JobDeletionComponent;

```
## OUTPUT
![image](https://github.com/Shavedha/Job-Portal/assets/93427376/77298168-4dff-411a-bfd8-c84b6c9ec9ee)
![image](https://github.com/Shavedha/Job-Portal/assets/93427376/1d9086cc-7a09-4de5-af4f-ea079a8f671a)

## RESULT
Thus a Job Portal is created successfully using React,SpringBoot and SQL.
