# Manav
import React, { useState } from 'react';
import { Eye, Download, Heart } from 'lucide-react';
import { CollageItem } from '../types';

interface CollageGridProps {
  collages: CollageItem[];
}

const CollageGrid: React.FC<CollageGridProps> = ({ collages }) => {
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {collages.map((collage) => (
        <CollageCard key={collage.id} collage={collage} />
      ))}
    </div>
  );
};

const CollageCard: React.FC<{ collage: CollageItem }> = ({ collage }) => {
  const [isHovered, setIsHovered] = useState(false);
  
  return (
    <div 
      className="group relative bg-white rounded-xl overflow-hidden shadow-md transition-all duration-300 hover:shadow-lg transform hover:-translate-y-1"
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      <div className={`relative ${getLayoutClass(collage.layout)}`}>
        {collage.photos.map((photo, index) => (
          <div 
            key={photo.id} 
            className={`overflow-hidden ${getPhotoClass(collage.layout, index, collage.photos.length)}`}
          >
            <img 
              src={photo.url} 
              alt={photo.alt}
              className="w-full h-full object-cover transition-transform duration-700 group-hover:scale-105"
            />
          </div>
        ))}
        
        <div className={`absolute inset-0 bg-gradient-to-t from-black/60 to-transparent transition-opacity duration-300 ${isHovered ? 'opacity-100' : 'opacity-0'}`} />
      </div>
      
      <div className="absolute bottom-0 left-0 right-0 p-4 text-white z-10">
        <h3 className="text-lg font-medium transition-transform duration-300 transform translate-y-0 group-hover:translate-y-0">{collage.title}</h3>
        <p className="text-sm text-white/80 mb-2">{formatDate(collage.created)}</p>
        
        <div className={`flex space-x-4 transition-all duration-300 ${isHovered ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-4'}`}>
          <button className="text-white hover:text-indigo-300 transition-colors">
            <Eye className="h-5 w-5" />
          </button>
          <button className="text-white hover:text-indigo-300 transition-colors">
            <Heart className="h-5 w-5" />
          </button>
          <button className="text-white hover:text-indigo-300 transition-colors">
            <Download className="h-5 w-5" />
          </button>
        </div>
      </div>
    </div>
  );
};

// Helper functions for layout classes
const getLayoutClass = (layout: string): string => {
  switch (layout) {
    case 'grid-2':
      return 'grid grid-cols-2 aspect-video';
    case 'split-2':
      return 'grid grid-cols-2 aspect-video';
    case 'grid-3':
      return 'grid grid-cols-3 aspect-video';
    case 'grid-4':
      return 'grid grid-cols-2 grid-rows-2 aspect-square';
    case 'feature-3':
      return 'grid grid-cols-2 grid-rows-2 aspect-video';
    default:
      return 'grid grid-cols-1 aspect-video';
  }
};

const getPhotoClass = (layout: string, index: number, total: number): string => {
  switch (layout) {
    case 'feature-3':
      return index === 0 ? 'row-span-2' : '';
    default:
      return '';
  }
};

const formatDate = (date: Date): string => {
  return date.toLocaleDateString('en-US', { 
    year: 'numeric',
    month: 'short',
    day: 'numeric'
  });
};

export default CollageGrid;
