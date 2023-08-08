[comment]: # (lmake_readme exclude start A)

# amafatt (2019-09)

**amazon bills to fatture.it**  
***version: 1.0  date: 2019-09-13 author: [bestia.dev](https://bestia.dev) repository: [GitHub](https://github.com/bestia-dev/amafatt)***  
![status](https://img.shields.io/badge/archived(2023)-yellow) 
![status](https://img.shields.io/badge/Not_Maintained-yellow) 
![Hits](https://bestia.dev/webpage_hit_counter/get_svg_image/775315520.svg)

A short description in Italian is in README_IT.md.  

[comment]: # (lmake_readme exclude end A)

## Documentation

Documentation generated from source code:  
<https://bestia-dev.github.io/amafatt/index.html>

## Rust, Wasm, Dodrio

The programming language of this application is not important.  
It is easy to rewrite the application in any other language if necessary.  
The important thing is the application logic and mapping of data.  
I will create the prototype in the Rust programming language. It will be
compiled in Wasm/WebAssembly that runs inside all modern browsers.  
The GUI interface will be in simple HTML updated with Dodrio Virtual Dom.  
There will not be necessary any special servers, JavaScript, npm, web-pack
or complicated css and html.  

## Development environment

Install the rust toolchain from  
<https://www.rust-lang.org/tools/install>.  
For wasm workflow tool installation on Windows:  
<https://rustwasm.github.io/wasm-pack/installer/>  
You will need also a simple static file server:  
`cargo install basic-http-server`  

## Source

Absolutely everything manually coded is in Rust language in the folder `/src/`.  
No manual JavaScript or HTML needed.  
The index.html file is a simple template.  
The pkg/mem.js file is generated by wasm-bindgen on building.  
I prepared a small simple css with css grid for styling based on w3.css.
No need for npm or web-pack.  
The proper way to do json would be with structs and vectors, but it is so much typing and retyping of datatypes and input fields - for small benefit. I will use whole json as a string. And there replace what should be replaced. Much simpler that way for this project. Even json objects are too complicated. I will use all that as simple strings.  

## Make

I prepared all the needed tasks/flows in cargo make.  
<https://github.com/sagiegurari/cargo-make>  
Install it simply with `cargo install --force cargo-make`  
`cargo make` - lists the possible available/public flows/tasks  
`cargo make dev` - builds the development version and runs the server and the browser  
`cargo make release` - builds the release version and runs the server and the browser  
The make script also lunches the `basic-http-server`and open the browser.  
To copy the content of readme.md files into doc comments of *.rs files I created this utility:  
<https://github.com/bestia-dev/lmake_readme>  
`cargo make doc` - prepare the doc comments and creates the docs folder  
TODO: could make a flow to publish to GitHub or to google vm or a task for increment version numbers.  

## Workflow

There is only one screen with big text fields for request and response from FattureInCloud.it.  
A menu will have buttons to create different requests.  
For importing data from Amazon there will be another text field where to copy/paste the content of the txt file from Amazon. No file uploads here because we don't really need/have a server.  
The button Extract order-id will create a vector of order-id in case there are multiples in the amazon txt. For every order-id there will be a new button to process the txt and create a json string as close as possible what the API needs. That json data is possible to manually edit if needed.  
The button `Send json` sends the json request to the API and fetch the response into the response field. Only one order-id should be sent at a time.
After that all needed editing, deleting, changing is made through the FattureInCloud.it user interface.  
TODO: after some time it can be modified to send all the order-id one by one automatically.

## reverse proxy  

Modern browsers don't allow cross origin fetch by default.  
The server must allow it explicitly with a special header `Access-Control-Allow-Origin=*`.  
As expected, the site FattureInCloud.it does not send this header.  
I wrote them to correct this, but I don't have any hope they will listen to me.  
The workaround is a reverse proxy that receives the original request,
sends it to FattureInCloud.it, receives the response, add this special header
and sends the response back to the browser.
I use nginx on my google cloud vm. To achieve this I added this lines
in the configuration file \etc\nginx\sites-available\default:  

```bash
 #region amafatt
  # example https://api.FattureInCloud.it/v1/richiesta/info

  # the trailing / after both of these lines means this route is not appended to the forwarding
  location /v1/ {
   proxy_pass https://api.FattureInCloud.it/v1/;
   proxy_buffering off;
   add_header  Access-Control-Allow-Origin *;
  }
 #endregion
```

All the requests will be now sent to <https://bestia.dev/v1/>.  

## Reference

<https://github.com/rustwasm/wasm-bindgen/tree/master/examples/fetch>  
<https://api.FattureInCloud.it/v1/documentation/dist/#!/Richiesta_generica/jsonrequest>  

[comment]: # (lmake_readme exclude start A)

## Complaints of an old man

For a simple fetch api I had to jump over hoops that are not to underestimate.  
JsValue, Promises, Futures, Closures, reference to functions, lifetimes,...  
I probably did not mention them all. Feeling like an absolute beginner. Feeling young again.  

## cargo crev reviews and advisory

It is recommended to always use [cargo-crev](https://github.com/crev-dev/cargo-crev)  
to verify the trustworthiness of each of your dependencies.  
Please, spread this info.  
On the web use this url to read crate reviews. Example:  
<https://web.crev.dev/rust-reviews/crate/num-traits/>  

## TODO

- show errors to user. Now they show only on console F12.


## Changelog  

2019-07-15 start, async fetch working, reverse proxy working.  
2019-07-21 app uploaded to bestia.dev/amafatt, ricevute lista & dettagli examples  
2019-07-22 csv read and replace into json  
2019-07-23 working version for the simplest amazon txt  
2019-07-24 css, style, disabled, 22% iva, calculate net price,country name from ISO  
2019-08-03 multirecord, multiproduct csv, country name in Italian  
2019-08-05 cod_iva=0 for 22%, prezzi_ivati=true then only brutto prices, api_key and uid of Max  
2019-08-09 The product name fetch from FattureInCloud, recognize spedizione type from price  
2019-08-10 State provincia, modifiable api_key and api_uid, saved in local storage  
2019-08-15 because of credentials, delete all git and GitHub history, instructions for credentials  
2019-08-19 lmake_readme inserts the content of readme.md into *.rs comments (for the docs) and use it in cargo-make doc  
2019-08-21 1.0.3 today-date  
2019-09-09 version 19.9.9. no need for semver for non-library, version in docs

## Open-source and free as a beer

My open-source projects are free as a beer (MIT license).  
I just love programming.  
But I need also to drink. If you find my projects and tutorials helpful, please buy me a beer by donating to my [PayPal](https://paypal.me/LucianoBestia).  
You know the price of a beer in your local bar ;-)  
So I can drink a free beer for your health :-)  
[Na zdravje!](https://translate.google.com/?hl=en&sl=sl&tl=en&text=Na%20zdravje&op=translate) [Alla salute!](https://dictionary.cambridge.org/dictionary/italian-english/alla-salute) [Prost!](https://dictionary.cambridge.org/dictionary/german-english/prost) [Nazdravlje!](https://matadornetwork.com/nights/how-to-say-cheers-in-50-languages/) 🍻

[//bestia.dev](https://bestia.dev)  
[//github.com/bestia-dev](https://github.com/bestia-dev)  
[//bestiadev.substack.com](https://bestiadev.substack.com)  
[//youtube.com/@bestia-dev-tutorials](https://youtube.com/@bestia-dev-tutorials)  

[comment]: # (lmake_readme exclude end A)
