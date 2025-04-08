# Gawad-Heneral-Film-Festival-2025.
import React, { useState, useRef } from "react";
import { Button } from "@/components/ui/button";
import { Card } from "@/components/ui/card";
import html2canvas from "html2canvas";
import QrReader from "react-qr-reader";

export default function GawadHeneralFilmFestival2025() {
  const [images, setImages] = useState([]);
  const [showScanner, setShowScanner] = useState(false);
  const stripRef = useRef(null);

  const handleScan = (data) => {
    if (data && images.length < 4 && !images.includes(data)) {
      setImages([...images, data]);
    }
  };

  const handleError = (err) => {
    console.error(err);
  };

  const downloadStrip = async () => {
    if (stripRef.current) {
      const canvas = await html2canvas(stripRef.current);
      const link = document.createElement("a");
      link.download = "Gawad-Heneral-4-Cuts.png";
      link.href = canvas.toDataURL();
      link.click();
    }
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-black via-gray-900 to-gray-800 flex flex-col items-center justify-center p-6 text-white">
      <h1 className="text-3xl font-extrabold mb-2 tracking-wide text-yellow-400">
        Gawad Heneral Film Festival 2025
      </h1>
      <p className="text-sm text-gray-300 mb-6 italic">Scan your QR to upload cinematic moments</p>

      {!showScanner && (
        <Button onClick={() => setShowScanner(true)} className="mb-4 bg-yellow-500 hover:bg-yellow-400 text-black font-bold">
          Scan QR Code
        </Button>
      )}

      {showScanner && (
        <div className="mb-4 bg-white rounded-xl p-2">
          <QrReader
            delay={300}
            onError={handleError}
            onScan={handleScan}
            style={{ width: "300px" }}
          />
        </div>
      )}

      <div
        ref={stripRef}
        className="flex flex-col space-y-2 bg-yellow-100 p-4 rounded-2xl shadow-2xl border-4 border-yellow-400"
        style={{ width: "220px" }}
      >
        {images.map((src, index) => (
          <img
            key={index}
            src={src}
            alt={`cut-${index}`}
            className="rounded-xl w-full object-cover border-2 border-yellow-400"
          />
        ))}
      </div>

      {images.length > 0 && (
        <Button onClick={downloadStrip} className="mt-6 bg-yellow-500 hover:bg-yellow-400 text-black font-bold">
          Download Your Film Strip
        </Button>
      )}
    </div>
  );
}
