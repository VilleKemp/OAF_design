# OAF design
This repository contains documentation used in desing and creation of OAF.

## OAF structure

OAF consists of 3 main parts, parser, request generator and fuzzer.

## Parser

Parser takes openapi spec as its input and parses through it generating API object. Structure of this object can be found from objects_in_oaf.md

## Request generator
Parses the API object and generates a Request object for each endpoint and method. Then it randomizes all the parameter values and sends it to the API. If it receives an acceptable response it will save this object as a valid request and continues on. 

## Fuzzer

Takes valid requests and fuzzes all the parameters.
