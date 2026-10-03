# API Security Checklist
 
[Leer en Español](README.es.md)
 
A practical checklist for reviewing REST API security during testing and during design/code reviews within an SSDLC. Organized around the **OWASP API Security Top 10 (2023 edition)**.
 
Each item includes: what to check, a test example, what evidence to keep, and how to mitigate it.
 
## Who it's for
- QA and testing roles who want to add security checks to their API testing.
- Development teams, to review design and code before a release.
- Anyone who needs a template for security tickets or reviews.
  
## How to use it
1. Copy [`checklist.md`](checklist.md) into the ticket, pull request, or review document.
2. Mark each item as **OK / FAIL / N/A** and note the affected endpoint.
3. Attach evidence (request, response, screenshot) and open a ticket for each FAIL.
4. Repeat the review whenever the design changes or new endpoints are added.
   
## Scope and disclaimer
- Test **only systems you own or have explicit authorization to test**.
- Examples use `https://api.example.com` as a fictional server.
- To practice safely, use intentionally vulnerable apps run locally, such as OWASP crAPI or OWASP Juice Shop.
  
## Repository structure
```
api-security-checklist/
├── README.md
├── README.es.md
├── checklist.md
├── LICENSE
└── examples/          (sample tickets and Postman collections, coming soon)
```
 
## Roadmap
- [ ] Vulnerability ticket examples (Jira format)
- [ ] Postman collection with the base tests
- [ ] SOAP section
      
## References
- [OWASP API Security Project](https://owasp.org/API-Security/)
  
## Author
**Luis Oñoro**, Telecommunications Engineer with an MSc in Cybersecurity, background in QA, automation, and security testing.
[LinkedIn](https://www.linkedin.com/in/luisonoro-cyber/)
 
## License
MIT
