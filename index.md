---
layout: page
---

Found a paywall? Paste the URL here and we'll generate (and copy to clipboard if possible) 
the [Edge Immersive Reader](https://support.microsoft.com/en-us/topic/use-immersive-reader-in-microsoft-edge-78a7a17d-52e1-47ee-b0ac-eff8539015e1) 
URL which can help you read the content without the paywall.

Te piden pagar para leer un articulo? Pega la URL aca y generaremos (y copiaremos al portapapeles si es posible)
la URL del [Edge Immersive Reader](https://support.microsoft.com/en-us/topic/use-immersive-reader-in-microsoft-edge-78a7a17d-52e1-47ee-b0ac-eff8539015e1)
que te permitirá leer el contenido sin pagar.

<!-- why this table has a order? -->
<table>
    <tr style="border: none !important; ">
        <td style="width: 100%;">
            <input id="url" onpaste="go()" oninput="go()" style="width: 100%;">
        </td>
        <td style="width: 1%">
            <button onclick="go()" class="btn btn-green">Copy</button>
        </td>
    </tr>
</table>

<p id="status" style="display: none;">
</p>

<p id="reader" style="display: none;">
</p>

<script>
    // on document loaded, check if the URL has a query parameter and if so, set the value of the input field
    document.addEventListener("DOMContentLoaded", function () {
        var urlParams = new URLSearchParams(window.location.search);
        var url = urlParams.get('url');
        if (url) {
            document.getElementById("url").value = url;
            setTimeout(() => {
                go(false);
            }, 50);
        }
    });

    function setStatus(text) {
        document.getElementById("status").innerHTML = text;
        if (text.length > 0) {
            document.getElementById("status").style.display = "block";
        } else {
            document.getElementById("status").style.display = "none";
        }
    }

    async function setReaderUrl(url, href, copy) {
        document.getElementById("reader").innerHTML = href;
        document.getElementById("reader").style.display = "block";
        if (copy) {
            if (navigator.clipboard) {
                try {
                    await navigator.clipboard.writeText(href);
                    setStatus("✅ Immersive Reader URL copied to clipboard!");
                    // push the URL to the browser history so that the user can share the URL
                    history.pushState({}, null, "?url=" + url);
                } catch (e) {
                    setStatus("❌ Error copying to clipboard");
                }
            } else {
                setStatus("❔ Clipboard unavailable. Please copy the URL manually.");
            }
        }
    }

    function go(copy = true) {
        setTimeout(async () => {
            var url = document.getElementById("url").value;
            // try parsing the url to see if it's a valid url
            try {
                var uri = new URL(url);
                // given this url: https://www.clarin.com/sociedad/alerta-gobierno-record-contagios-sifilis-riesgo-inedito-transmision_0_0zjIh8EYPm.html
                // url should be assigned to https_www.clarin.com
                var base = uri.protocol.replace(":", "") + "_" + uri.hostname;
                var href = "read://" + base + "?url=" + encodeURIComponent(document.getElementById("url").value);
                await setReaderUrl(url, href, copy);
            } catch (e) {
                document.getElementById("reader").style.display = "none";
                if (url.length == 0) {
                    setStatus("");
                } else {
                    setStatus("Invalid URL");

                }
            }
        }, 0);
    }

</script>