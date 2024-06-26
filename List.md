// src/pages/index.js
import React, { useState, useEffect } from 'react';
import Head from 'next/head';
import { ExternalLink } from 'lucide-react';

const ScriptItem = ({ title, url, characters }) => {
  const [showPreview, setShowPreview] = useState(false);
  const [preview, setPreview] = useState('');

  useEffect(() => {
    if (showPreview && !preview) {
      fetchPreview();
    }
  }, [showPreview]);

  const fetchPreview = async () => {
    try {
      const response = await fetch(`/api/preview?url=${encodeURIComponent(url)}`);
      const data = await response.json();
      setPreview(data.preview);
    } catch (error) {
      console.error('Failed to fetch preview:', error);
      setPreview('プレビューを読み込めませんでした。');
    }
  };

  return (
    <div 
      className="relative border-b py-2"
      onMouseEnter={() => setShowPreview(true)}
      onMouseLeave={() => setShowPreview(false)}
    >
      <div className="flex justify-between items-center">
        <h3 className="font-semibold">{title}</h3>
        <div className="flex items-center">
          <span className="mr-2 text-sm text-gray-600">登場人数: {characters}</span>
          <a 
            href={url} 
            target="_blank" 
            rel="noopener noreferrer"
            className="text-blue-500 hover:text-blue-700"
          >
            <ExternalLink size={16} />
          </a>
        </div>
      </div>
      {showPreview && (
        <div className="absolute z-10 left-0 mt-2 p-4 bg-white border rounded shadow-lg w-64 max-h-48 overflow-y-auto">
          <h4 className="font-bold mb-2">{title}</h4>
          <p className="text-sm">{preview || 'プレビューを読み込み中...'}</p>
        </div>
      )}
    </div>
  );
};

const ScriptList = ({ scripts }) => {
  return (
    <div className="max-w-2xl mx-auto p-4">
      <h2 className="text-2xl font-bold mb-4">一節脚本</h2>
      {scripts.map((script, index) => (
        <ScriptItem key={index} {...script} />
      ))}
    </div>
  );
};

const sampleScripts = [
  { 
    title: "暇", 
    url: "https://manatsusouth.tumblr.com/post/746741738873487360/%E4%B8%80%E7%AF%80%E8%84%9A%E6%9C%AC%EF%BC%91-%E6%9A%87", 
    characters: 2
  },
  { 
    title: "熱", 
    url: "https://manatsusouth.tumblr.com/post/746806556871540736/%E4%B8%80%E7%AF%80%E8%84%9A%E6%9C%AC%EF%BC%92-%E7%86%B1", 
    characters: 2
  },
  { 
    title: "枕", 
    url: "https://manatsusouth.tumblr.com/post/746872895150686208/%E4%B8%80%E7%AF%80%E8%84%9A%E6%9C%AC%EF%BC%93-%E6%9E%95", 
    characters: 2
  },
  // ... 他の脚本データ
];

export default function Home() {
  return (
    <div>
      <Head>
        <title>一節脚本リスト</title>
        <meta name="description" content="一節脚本のリストとプレビュー" />
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <main>
        <ScriptList scripts={sampleScripts} />
      </main>
    </div>
  );
}
