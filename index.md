---
layout: page
---

Found a paywall? Paste the URL here and we'll generate (and copy to clipboard if possible) 
the [Edge Immersive Reader](https://support.microsoft.com/en-us/topic/use-immersive-reader-in-microsoft-edge-78a7a17d-52e1-47ee-b0ac-eff8539015e1) 
URL which can help you read the content without the paywall.

Te piden pagar para leer un articulo? Pega la URL aca y generaremos (y copiaremos al portapapeles si es posible)
la URL del [Edge Immersive Reader](https://support.microsoft.com/en-us/topic/use-immersive-reader-in-microsoft-edge-78a7a17d-52e1-47ee-b0ac-eff8539015e1)
que te permitirá leer el contenido sin pagar.

<input id="url" onpaste="go()" oninput="go()" class="border" style="width: 100%">
<p id="status">
</p>

<script>
    function setStatus(text) {
        document.getElementById("status").innerHTML = text;
    }

    function go() {
        setTimeout(async () => {
            var url = document.getElementById("url").value;
            // try parsing the url to see if it's a valid url
            try {
                var uri = new URL(url);
                // given this url: https://www.clarin.com/sociedad/alerta-gobierno-record-contagios-sifilis-riesgo-inedito-transmision_0_0zjIh8EYPm.html
                // url should be assigned to https_www.clarin.com
                var url = uri.protocol.replace(":", "") + "_" + uri.hostname;
                var href = "read://" + url + "?url=" + encodeURIComponent(document.getElementById("url").value);

                if (navigator.clipboard) {
                    try {
                        await navigator.clipboard.writeText(href);
                        setStatus("✅ Immersive Reader URL copied to clipboard! <p>" + href + "</p>");
                    } catch (e) {
                        setStatus(href);
                    }
                } else {
                    setStatus(href);
                }
            } catch (e) {
                if (url.length == 0) {
                    setStatus("");
                } else {
                    setStatus("Invalid URL");
                }
            }
        }, 0);
    }

</script>