<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    // Data van landen met hun bijbehorende waarden (test data)
    const landen = [
        { name: 'Nederland', value: 82 },
        { name: 'Marokko', value: 74 },
        { name: 'Frankrijk', value: 83 },
        { name: 'Verenigde Staten', value: 77 },
        { name: 'China', value: 78 },
        { name: 'Duitsland', value: 81 },
        { name: 'Italië', value: 83 },
        { name: 'Spanje', value: 83 },
        { name: 'Brazilië', value: 75 },
        { name: 'Japan', value: 85 },
        { name: 'India', value: 70 },
        { name: 'Canada', value: 82 },
        { name: 'Zuid-Afrika', value: 65 },
        { name: 'Australië', value: 83 },
        { name: 'Rusland', value: 71 },
        { name: 'Verenigde Arabische Emiraten', value: 78 },
        { name: 'Algerije', value: 77 },
        { name: 'Argentinië', value: 76 },
        { name: 'België', value: 82 },
        { name: 'Slowakije', value: 77 },
        { name: 'Mexico', value: 76 },
        { name: 'Somalië', value: 57 },
        { name: 'Chili', value: 80 },
        { name: 'Zweden', value: 83 },
        { name: 'Noorwegen', value: 83 },
        { name: 'Finland', value: 82 },
        { name: 'Denemarken', value: 81 },
        { name: 'Zwitserland', value: 84 },
        { name: 'Portugal', value: 81 },
        { name: 'Griekenland', value: 81 },
        { name: 'Turkije', value: 77 },
        { name: 'Vietnam', value: 75 },
        { name: 'Thailand', value: 77 },
        { name: 'Filipijnen', value: 71 },
        { name: 'Pakistan', value: 66 },
        { name: 'Afghanistan', value: 64 },
        { name: 'Oekraïne', value: 71 },
        { name: 'Kazachstan', value: 73 },
        { name: 'Oezbekistan', value: 72 },
        { name: 'Irak', value: 70 },
        { name: 'Iran', value: 77 },
        { name: 'Saudi-Arabië', value: 75 },
        { name: 'Egypte', value: 71 },
        { name: 'Nigeria', value: 55 },
        { name: 'Ethiopië', value: 65 },
        { name: 'Kenia', value: 66 },
        { name: 'Tanzania', value: 65 },
        { name: 'Angola', value: 62 },
        { name: 'Ghana', value: 64 },
        { name: 'Venezuela', value: 72 },
        { name: 'Colombia', value: 76 },
        { name: 'Peru', value: 77 },
        { name: 'Bolivia', value: 70 },
        { name: 'Paraguay', value: 73 },
        { name: 'Uruguay', value: 78 },
        { name: 'Cuba', value: 79 },
        { name: 'Haïti', value: 64 },
        { name: 'Dominicaanse Republiek', value: 74 },
        { name: 'Jamaica', value: 75 },
        { name: 'Zuid-Korea', value: 84 },
        { name: 'Noord-Korea', value: 73 },
        { name: 'Maleisië', value: 76 },
        { name: 'Indonesië', value: 72 },
        { name: 'Nieuw-Zeeland', value: 82 }
    ];

    let filteredData = [...landen]; 
    let chartContainer; // Referentie naar de SVG-container

    // Functie om de grafiek te updaten
    function updateChart(data) {
        const barHeight = 27; // Hoogte van elke balk
        const barSpacing = 40; // Ruimte tussen de balken
        const svgWidth = 1650; // Breedte van de SVG
        const svgHeight = data.length * barSpacing + 50; // Dynamische hoogte op basis van aantal data

        // Schaal voor de X-as op basis van maximale waarde
        const xScale = d3.scaleLinear()
            .domain([0, d3.max(data, d => d.value)])
            .range([0, svgWidth - 400]);

        // Stelt de grootte van de SVG in
        d3.select(chartContainer)
            .select('svg')
            .attr('width', svgWidth)
            .attr('height', svgHeight);

        // Voegt balken toe aan de grafiek
        d3.select(chartContainer)
            .select('.bars')
            .selectAll('rect')
            .data(data)
            .join('rect')
            .attr('height', barHeight)
            .attr('fill', 'black')
            .attr('width', (d) => xScale(d.value))
            .attr('y', (d, i) => i * barSpacing)
            .attr('x', 250);

        // Voegt labels met landennamen toe
        d3.select(chartContainer)
            .select('.labels')
            .selectAll('text.name-label')
            .data(data)
            .join('text')
            .attr('class', 'name-label')
            .attr('x', 0)
            .attr('y', (d, i) => i * barSpacing + barHeight / 2)
            .attr('dy', '0.35em')
            .attr('fill', 'black')
            .attr('text-anchor', 'start')
            .text(d => d.name);

        // Voegt waardelabels toe aan de balken
        d3.select(chartContainer)
            .select('.labels')
            .selectAll('.value-label')
            .data(data)
            .join('text')
            .attr('class', 'value-label')
            .attr('fill', 'white')
            .attr('x', d => xScale(d.value) + 230)
            .attr('y', (d, i) => i * barSpacing + barHeight / 2)
            .attr('dy', '0.35em')
            .attr('text-anchor', 'end')
            .text(d => d.value);
    }

    // Functie om de data te sorteren
    function sortChart(order) {
        if (order === 'asc') {
            filteredData.sort((a, b) => d3.ascending(a.value, b.value));
        } else if (order === 'desc') {
            filteredData.sort((a, b) => d3.descending(a.value, b.value));
        }
        updateChart(filteredData);
    }

    // Functie om te zoeken in de data
    function searchCountries(query) {
        filteredData = landen.filter(d => d.name.toLowerCase().includes(query.toLowerCase()));
        updateChart(filteredData);
    }

    // Initialiseert de grafiek bij het laden van de pagina
    onMount(() => {
        updateChart(filteredData);
    });
</script>

<style>
    .chart-container {
        position: relative;
        width: auto;
        max-width: 100%;
        overflow-x: auto;
        font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
        font-size: 1.2em;
        margin: 2em;
    }

    .name-label {
        font-size: 14px;
        font-weight: bold;
    }

    button {
        margin: 5px;
        padding: 10px;
        font-size: 14px;
        cursor: pointer;
    }

    input {
        margin: 5px;
        padding-top: 20px;
        padding-bottom: 20px;
        padding-left: 100px;
        padding-right: 100px;
        font-size: 14px;
        border: fill, solid, 80px, black;
    }


</style>

<div>
    <!-- =zoekbalk -->
    <input
        type="text"
        placeholder="Zoek een land..."
        on:input={(e) => searchCountries(e.target.value)}
    />

    <!-- Sorteer knoppen -->
    <button on:click={() => sortChart('asc')}>Sorteer Oplopend</button>
    <button on:click={() => sortChart('desc')}>Sorteer Aflopend</button>

    <!-- grafiek -->
    <div class="chart-container" bind:this={chartContainer}>
        <svg>
            <g class="bars"></g>
            <g class="labels"></g>
        </svg>
    </div>
</div>
