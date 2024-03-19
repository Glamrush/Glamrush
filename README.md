- 👋 Hi, I’m @Glamrush
- 👀 I’m interested in photoshop scripting
- 🌱 I’m currently learning and i'm so smart like a cow in coding :-)
- 💞️ I’m looking to collaborate on whatever
- 📫 How to reach me kurt.t@curtsjam.com
- 😄 Pronouns: Kurt
- ⚡ Fun fact: down to earth

My code where i'm struggling with:


#target photoshop

function main() {
    var originalUnits = app.preferences.rulerUnits;
    app.preferences.rulerUnits = Units.INCHES;

    var sourceFile = File.openDialog("Selecteer de originele afbeelding");
    if (!sourceFile) {
        alert("Geen bestand geselecteerd.");
        return;
    }

    var doc;
    try {
        doc = open(sourceFile);
    } catch (e) {
        alert("Kon het bestand niet openen: " + e);
        return;
    }

    var destinationFolder = Folder.selectDialog("Selecteer de doelmap voor de opgeslagen bestanden");
    if (!destinationFolder) {
        alert("Geen doelmap geselecteerd.");
        doc.close(SaveOptions.DONOTSAVECHANGES);
        return;
    }

    var sizes = [
        {width: 23.39, height: 16.54, name: "A2"},
	{width: 30, height: 20, name: "20x30inch__51x76cm__3x4AR"},
        {width: 24, height: 18, name: "18x24inch__46x61cm__3x4AR"},
        {width: 20, height: 16, name: "16x20inch__41x51cm__4x5AR"},
        {width: 18, height: 12, name: "12x18inch__30x46cm__3x4AR"},
        {width: 16, height: 12, name: "12x16inch__30x41cm__3x4AR"},
        {width: 14, height: 11, name: "11x14inch__28x36cm__11x14AR"},
        {width: 16.54, height: 11.69, name: "A3"},
        {width: 12, height: 9, name: "9x12inch__23x30cm__3x4AR"},
        {width: 10, height: 8, name: "8x10inch__20x25cm__4x5AR"},
        {width: 11.69, height: 8.27, name: "A4"},
        {width: 8, height: 6, name: "6x8inch__15x20cm__3x4AR"},
        {width: 8.27, height: 5.83, name: "A5"},
        {width: 7, height: 5, name: "5x7inch__13x18cm__5x7AR"},
        {width: 6, height: 4, name: "4x6inch_10x15cm_2x3AR"},
        {width: 5.83, height: 4.13, name: "A6"}
        // Voeg hier meer maten toe...
    ];

    for (var i = 0; i < sizes.length; i++) {
        var size = sizes[i];
        centeredCrop(doc, size.width, size.height, size.name + "__" + (i + 1), destinationFolder);
    }

    doc.close(SaveOptions.DONOTSAVECHANGES);
    app.preferences.rulerUnits = originalUnits;
}

function centeredCrop(doc, targetWidth, targetHeight, name, destinationFolder) {
    var bounds = calculateCropBounds(doc.width.value, doc.height.value, targetWidth, targetHeight);
    doc.crop(bounds, 0, 300); // 0 is the angle, 300 is the resolution (dpi)

    var fileName = name + ".jpg";
    var file = new File(destinationFolder + "/" + fileName);
    var jpegOptions = new JPEGSaveOptions();
    jpegOptions.quality = 12;
    doc.saveAs(file, jpegOptions, true, Extension.LOWERCASE);

    // Undo the crop to revert to the original image for the next iteration
    var lastHistoryState = doc.historyStates.length - 1;
    doc.activeHistoryState = doc.historyStates[lastHistoryState - 1];
}

function calculateCropBounds(docWidth, docHeight, targetWidth, targetHeight) {
    // Bereken het centrum van de huidige afbeelding
    var centerX = docWidth / 2;
    var centerY = docHeight / 2;

    // Bereken de crop coördinaten om de crop rond het centrum te plaatsen
    var x1 = centerX - targetWidth / 2;
    var y1 = centerY - targetHeight / 2;
    var x2 = centerX + targetWidth / 2;
    var y2 = centerY + targetHeight / 2;

    return [x1, y1, x2, y2];
}

main();





<!---
Glamrush/Glamrush is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
