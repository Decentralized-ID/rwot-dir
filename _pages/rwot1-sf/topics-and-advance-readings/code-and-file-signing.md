---
title: "Blockchain-based Trust for Software Components"
permalink: rwot1-sf/topics-and-advance-readings/code-and-file-signing/
sidebar:
  - title: RWoT1-SF
    nav: rwot1
  - title: "Rebooting the Web of Trust"
    nav: rwotnav
authors:
  - "Sean Gilligan"
gitlink: "https://github.com/WebOfTrustInfo/rwot1-sf/blob/master/topics-and-advance-readings/code-and-file-signing.adoc"

---

*by Sean Gilligan <https://github.com/msgilligan>*
v0.1
:description: Rough draft of topic paper for Rebooting Web of Trust.

== Zooko's Triangle Stalks the Bazaar

A computing device cannot be secure if an attacker is able to surreptitiously insert malicious code on it. Current solutions for signing and verifying computer software -- both source code and binaries -- generally rely on the same flawed, hierarchical systems as our network connections do.

Blockchain technology allows us to sidestep Zooko's Triangle <<zooko>> and provide systems of identification that are decentralized, secure, and human-meaningful. Today's dominant code signing and verification technologies rely on centralized certificate authorities and/or centralized hardware or operating system vendors. These solutions are typically oriented toward end-user installation of applications and do not address the needs of software developers who increasingly rely on externally developed components in both source and binary form. Open Source Software has moved us in the direction of the Bazaar <<catb>> and created an incredibly powerful and diverse supply chain, but has brought with it new risks.

Language-specific software component repositories such as Maven, NPM, RubyGems, and CocoaPods are not as secure as they should be and yet most developers have no alternative but to assume their component downloads have not been tampered with. In the rare circumstances where developers are validating their downloaded components they're using homegrown validation tools and often manually verifying hashes. Typically file hashes are stored on the same server as the hashed file to be validated. If the download is compromised, the hash will  be too. Developers need a way of sending and receiving decentralized, secure, and human meaningful information that can be used to secure software components and applications.


== Blockchain-based ID Systems Can Help

As blockchain-based identity solutions become increasingly viable and search for early adopters, they can be applied to the software development process itself.

Namecoin is (one of?) the first blockchain-based identity solutions and currently provides enough functionality to solve some of the major issues involved in secure software distribution. Blockchain-agnostic middleware like DNSChain <<dnschain>> can provide servers and APIs for integrating with software build tools to provide the decentralized identity and other functions that are needed. Developing tools that are blockchain-agnostic will reassure tool developers that they can migrate to another blockchain based upon future adoption and technology trends.

One area of concern is "blockchain bloat", but it is unclear whether file signatures would be stored on a blockchain and, if so, how many would really need to be store. There has been preliminary discussion within the Namecoin community about blockchain-based signing/hashing of file contents although simply using a blockchain user id or domain id may be enough to significantly improve some of the current pain points.

== Proposed directions

We propose discussing these issues at the conference and unless presented with more attractive options, proceed with developing a working prototype for the Java ecosystem that uses Namecoin and DNSChain. Why Java? It is large, it generally distributes binaries rather than sources, it is clearly in need of solution, and (this author) knows it best.

== Java & Maven Repositories

For years the default option for the leading Java repository, http://central.sonatype.org[Maven Central], was to use HTTP rather than HTTPS. This weakness was common knowledge, but generally ignored. In 2014 an exploit was demonstrated <<veytsman>> that used a MITM attack to insert unwanted code into any downloaded JAR file. Shortly thereafter https became the default option. However, there are still no reliable, automated methods for ensuring that files weren't corrupted on the Maven Central servers themselves <<jbaruch1>>. https://bintray.com/bintray/jcenter[JCenter] provides additional identity information about component developers but does not provide a truly decentralized solution <<jbaruch2>>.

Maven is the build tool that standardized the Maven repository format. Currently the https://maven.apache.org/enforcer/maven-enforcer-plugin/[Maven Enforcer Plugin] and the https://github.com/gary-rowe/BitcoinjEnforcerRules[BitcoinJEnforcerRules Plugin] are available for validating file hashes <<bitcoinj>>. Gradle is a newer build-tool that has been gaining in popularity and is now the standard build tool for the Android platform. The https://github.com/WhisperSystems/gradle-witness[Gradle Witness Plugin] seems to be the best (only) choice for validating downloads.

Given the author's preference for and familiarity with Gradle, it is likely that the Gradle Witness Plugin will be the starting point for the project.


== Issues

The issues are grouped into short-term (roughly corresponding with the coming "Rebooting the Web of Trust Hackathon") and longer-term issues.

=== Short-term

* It looks like Namecoin is going to be the first implementation, but feedback is welcome.
* Which namespaces will be used? `d/`, `id/`, or both?
* Integration with Gradle and Maven build tools (most likely via plugins)
* Focus on new releases that can use new methods and tools
* Other languages welcome if Hackathon volunteers.

=== Longer-term

* How to validate older, existing releases.
* Tools for languages and repositories other than Java
* How to handle name expiration
* Solutions for blockchains other than Namecoin
* Hierarchical authority and delegation for larger organizations
* Multi-signature key management (both blockchain address and code signing key)
* Code-signing certificate issuance and revocation

== Appendix: Language-Specific Tools & Software Repositories

This section will be updated as we collect additional information on the tools and repositories of other languages.

=== Ruby

* https://rubygems.org[RubyGems]

=== JavaScript (Node.js)

* https://www.npmjs.com/[NPM]
* http://bower.io/[Bower]

Early work on using hashes for validation of scripts loaded from CDNs: https://hacks.mozilla.org/2015/09/subresource-integrity-in-firefox-43/

=== Objective-C/Swift

* https://cocoapods.org/[CocoaPods]

== Bibliography

[bibliography]
- [[[zooko]]] Wilcox-O'Hearn, Zooko, "Names: Distributed, Secure, Human-Readable: Choose Two", 2001, http://web.archive.org/web/20011020191610/http://zooko.com/distnames.html

- [[[catb]]] Raymond, Eric Steven, "The Cathedral and the Bazaar", 2000, http://www.catb.org/esr/writings/cathedral-bazaar/introduction/

- [[[veytsman]]] Veytsman, Max, "How to take over the computer of any Java (or Clojure or Scala) developer", http://blog.ontoillogical.com/blog/2014/07/28/how-to-take-over-any-java-developer/

- [[[jbaruch1]]] Sadogursky, Baruch, "Feel secure with SSL? Think again.", Aug 14, 2014, http://blog.bintray.com/2014/08/04/feel-secure-with-ssl-think-again/

- [[[jbaruch2]]] Sadogursky, Baruch, "Feeling secure with Bintray downloads", May 14, 2015, http://blog.bintray.com/2015/05/14/feeling-secure-with-bintray-downloads/

- [[[bitcoinj]]] "How to depend on bitcoinj with Maven using projects" https://bitcoinj.github.io/using-maven




