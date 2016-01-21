package com.sdsu.app;


import javax.swing.*;
import java.io.IOException;
import java.awt.event.*;
import java.awt.*;
import com.esri.mo2.ui.bean.*;
import com.esri.mo2.ui.tb.ZoomPanToolBar;
import com.esri.mo2.ui.tb.SelectionToolBar;
import com.esri.mo2.ui.ren.LayerProperties;
import com.esri.mo2.cs.geom.Envelope;
import javax.swing.table.AbstractTableModel;
import javax.swing.table.TableColumn;
import com.esri.mo2.data.feat.*;
import com.esri.mo2.map.dpy.FeatureLayer;
import com.esri.mo2.file.shp.*;
import com.esri.mo2.map.dpy.Layerset;
import com.esri.mo2.ui.bean.Tool;
import java.util.ArrayList;
import java.io.*;
import sun.audio.*;
import javax.sound.sampled.AudioFormat;
import javax.sound.sampled.AudioInputStream;
import javax.sound.sampled.AudioSystem;
import javax.sound.sampled.Clip;
import javax.sound.sampled.DataLine;
import javax.sound.sampled.LineEvent;
import javax.sound.sampled.LineListener;
import javax.sound.sampled.LineUnavailableException;
import javax.sound.sampled.UnsupportedAudioFileException;


public class MyAssign extends JFrame
{
  static Map map = new Map();
  static boolean fullMap = true;
  Legend legend;
  Legend legend2;
  Layer layer = new Layer();
  Layer layer2 = new Layer();
  


  static com.esri.mo2.map.dpy.Layer layer4;
  com.esri.mo2.map.dpy.Layer activeLayer;
  int activeLayerIndex; 
  JMenuBar mbar = new JMenuBar();
  JMenu file = new JMenu("File");
  JMenuItem addlyritem = new JMenuItem("add layer",new ImageIcon("addtheme.gif"));
  JMenuItem remlyritem = new JMenuItem("remove layer",new ImageIcon("delete.gif"));
  JMenuItem propsitem = new JMenuItem("Legend Editor",new ImageIcon("properties.gif"));
  Toc toc = new Toc();
  String s1 = "C:\\esri\\MOJ20\\Samples\\Data\\World\\country.shp";
  String s2 = "C:\\Users\\Bushan\\Desktop\\gis project\\points\\points.shp";
  String datapathname = "";
  String legendname = "";
  ZoomPanToolBar zptb = new ZoomPanToolBar();
  static SelectionToolBar stb = new SelectionToolBar();
  JToolBar jtb = new JToolBar();
  ComponentListener complistener;
  JLabel statusLabel = new JLabel("status bar    LOC");
  java.text.DecimalFormat df = new java.text.DecimalFormat("0.000");
  JPanel myjp = new JPanel();
  JPanel myjp2 = new JPanel();
  JButton pointer = new JButton(new ImageIcon("pointer.gif"));
  JButton hotjb = new JButton(new ImageIcon("src\\hotlink.gif"));
  Arrow arrow = new Arrow();
  ActionListener lis;
  ActionListener layerlis;
  TocAdapter mytocadapter;
  Toolkit tk = Toolkit.getDefaultToolkit();
  Image bolt = tk.getImage("src\\hotlink.gif");  // 16x16 gif file
  java.awt.Cursor boltCursor = tk.createCustomCursor(bolt,new Point(6,30),"bolt");
  MyPickAdapter picklis = new MyPickAdapter();
  Identify hotlink = new Identify(); //the Identify class implements a PickListener,
 
  static String selected_country = null;
  static String region = null;
  static String capital_city = null;

  class MyPickAdapter implements PickListener
  {   //implements hotlink
    public void beginPick(PickEvent pe){};
    // this fires even when you click outside the states layer
    public void endPick(PickEvent pe){}
    public void foundData(PickEvent pe)
    {
      //fires only when a layer feature is clicked
      FeatureLayer flayer2 = (FeatureLayer) pe.getLayer();
      com.esri.mo2.data.feat.Cursor c = pe.getCursor();
      Feature f = null;
      Fields fields = null;

      if (c != null)
        f = (Feature)c.next();

      fields = f.getFields();
      System.out.println("****" + fields);

      selected_country = (String)f.getValue(1);
      region = (String)f.getValue(5);
      capital_city = (String)f.getValue(7);
		System.out.println("Checkpoint :: :: ::" + selected_country);
      try {
    		HotPick hotpick = new HotPick();//opens dialog window with Duke in it
    		hotpick.setVisible(true);
          } catch(Exception e){}
    }
  };

  static Envelope env;

  public MyAssign()
  {
    super("My Assignment");
    this.setSize(1000,500);
    zptb.setMap(map);
    stb.setMap(map);
    setJMenuBar(mbar);

    ActionListener lisZoom = new ActionListener()
    {
      public void actionPerformed(ActionEvent ae)
      {
        fullMap = false;
      }
    }; // can change a boolean here

    ActionListener lisFullExt = new ActionListener()
    {
    	public void actionPerformed(ActionEvent ae)
    	{
			fullMap = true;
		}
	};
    		// next line gets ahold of a reference to the zoomin button
    JButton zoomInButton = (JButton)zptb.getActionComponent("ZoomIn");
    JButton zoomFullExtentButton = (JButton)zptb.getActionComponent("ZoomToFullExtent");
    JButton zoomToSelectedLayerButton = (JButton)zptb.getActionComponent("ZoomToSelectedLayer");
    zoomInButton.addActionListener(lisZoom);
    zoomFullExtentButton.addActionListener(lisFullExt);
    zoomToSelectedLayerButton.addActionListener(lisZoom);

    complistener = new ComponentAdapter ()
    {
    	public void componentResized(ComponentEvent ce)
    	{
    		if(fullMap)
    		{
    			map.setExtent(env);
    			map.zoom(1.0);    //scale is scale factor in pixels
    			map.redraw();
    		}
    	}
    };
    addComponentListener(complistener);
    lis = new ActionListener()
    {
    	public void actionPerformed(ActionEvent ae)
    	{
    		Object source = ae.getSource();

      		if (source == hotjb)
      		{
				hotlink.setCursor(boltCursor);
        		map.setSelectedTool(hotlink);
      		}
      		else if(source == pointer)
      		{
      			map.setSelectedTool(arrow);
      		}
    	}
    };

    layerlis = new ActionListener()
    {
    	public void actionPerformed(ActionEvent ae)
    	{
      		Object source = ae.getSource();
      		if (source instanceof JMenuItem)
      		{
        		String arg = ae.getActionCommand();
        		if(arg == "add layer")
        		{
          			try {
            			AddLyrDialog aldlg = new AddLyrDialog();
            			aldlg.setMap(map);
            			aldlg.setVisible(true);
          				} catch(IOException e){}
				}

				else if(arg == "remove layer")
				{
  					try {
    					com.esri.mo2.map.dpy.Layer dpylayer =
   						legend.getLayer();
    					map.getLayerset().removeLayer(dpylayer);
    					map.redraw();
    					remlyritem.setEnabled(false);
    					propsitem.setEnabled(false);
    					stb.setSelectedLayer(null);
    					zptb.setSelectedLayer(null);
    					stb.setSelectedLayers(null);
  					} catch(Exception e) {}
				}

				else if(arg == "Legend Editor")
				{
      				LayerProperties lp = new LayerProperties();
       				lp.setLegend(legend);
       				lp.setSelectedTabIndex(0);
       				lp.setVisible(true);
				}

      		}
    	}
    };

    toc.setMap(map);
    mytocadapter = new TocAdapter()
    {
      public void click(TocEvent e)
      {
    		legend = e.getLegend();
        	activeLayer = legend.getLayer();
        	stb.setSelectedLayer(activeLayer);
        	zptb.setSelectedLayer(activeLayer);
        	// get active layer index for promote and demote
        	activeLayerIndex = map.getLayerset().indexOf(activeLayer);
        	// layer indices are in order added, not toc order.
        	com.esri.mo2.map.dpy.Layer[] layers = {activeLayer};
        	hotlink.setSelectedLayers(layers);// replaces setToc from MOJ10
        	remlyritem.setEnabled(true);
        	propsitem.setEnabled(true);
      }
    };

    map.addMouseMotionListener(new MouseMotionAdapter()
    {
      	public void mouseMoved(MouseEvent me)
      	{
			com.esri.mo2.cs.geom.Point worldPoint = null;
			if (map.getLayerCount() > 0)
			{
  				worldPoint = map.transformPixelToWorld(me.getX(),me.getY());
  				String s = "X:"+df.format(worldPoint.getX())+" "+
             				"Y:"+df.format(worldPoint.getY());
  				statusLabel.setText(s);
        	}

        	else
          		statusLabel.setText("X:0.000 Y:0.000");
      	}
    }
    );

    toc.addTocListener(mytocadapter);
    remlyritem.setEnabled(false); // assume no layer initially selected
    propsitem.setEnabled(false);
    addlyritem.addActionListener(layerlis);
    remlyritem.addActionListener(layerlis);
    propsitem.addActionListener(layerlis);
    file.add(addlyritem);
    file.add(remlyritem);
    file.add(propsitem);
    mbar.add(file);
    hotlink.addPickListener(picklis);
    hotlink.setPickWidth(5);// sets tolerance for hotlink clicks
    hotjb.addActionListener(lis);
    hotjb.setToolTipText("hotlink tool--click somthing to maybe see a picture");
    pointer.addActionListener(lis);
    pointer.setToolTipText("Return back to the pointer");
    jtb.add(hotjb);
    jtb.add(pointer);
    myjp.add(jtb);
    myjp.add(zptb);
    myjp.add(stb);
    myjp2.add(statusLabel);
    getContentPane().add(map, BorderLayout.CENTER);
    getContentPane().add(myjp,BorderLayout.NORTH);
    getContentPane().add(myjp2,BorderLayout.SOUTH);
    addShapefileToMap(layer,s1);
    addShapefileToMap(layer2,s2);
    getContentPane().add(toc, BorderLayout.WEST);
  }

  private void addShapefileToMap(Layer layer,String s)
  {
    String datapath = s;
    layer.setDataset("0;" + datapath);
    map.add(layer);
  }

  public static void main(String[] args)
  {
    MyAssign qstart = new MyAssign();
    qstart.addWindowListener(new WindowAdapter()
    {
      public void windowClosing(WindowEvent e)
      {
        System.exit(0);
      }
    }
    );

    qstart.setVisible(true);
    env = map.getExtent();
  }


}// The MYAssign class closes


class Arrow extends Tool
{
  public void mouseClicked(MouseEvent me)
  {}
}
class AddLyrDialog extends JDialog
{
  Map map;
  ActionListener lis;
  JButton ok = new JButton("OK");
  JButton cancel = new JButton("Cancel");
  JPanel panel1 = new JPanel();
  com.esri.mo2.ui.bean.CustomDatasetEditor cus = new com.esri.mo2.ui.bean.
  CustomDatasetEditor();

  AddLyrDialog() throws IOException
  {
    setBounds(50,50,520,430);
    setTitle("Select a theme/layer");
    addWindowListener(new WindowAdapter()
    {
      public void windowClosing(WindowEvent e)
      {
        setVisible(false);
      }
    }
    );

    lis = new ActionListener()
    {
      public void actionPerformed(ActionEvent ae)
      {
        Object source = ae.getSource();
        if (source == cancel)
          setVisible(false);
        else
        {
          try {
            	setVisible(false);
            	map.getLayerset().addLayer(cus.getLayer());
            	map.redraw();
            	if (MyAssign.stb.getSelectedLayers() != null)
            	{
               	//	MyAssign.promoteitem.setEnabled(true);
               	}
  			  } catch(IOException e){}
		}
      }
    };

    ok.addActionListener(lis);
    cancel.addActionListener(lis);
    getContentPane().add(cus,BorderLayout.CENTER);
    panel1.add(ok);
    panel1.add(cancel);
    getContentPane().add(panel1,BorderLayout.SOUTH);
  }

  public void setMap(com.esri.mo2.ui.bean.Map map1)
  {
    map = map1;
  }
}


class HotPick extends JDialog
{
  String selected_country = MyAssign.selected_country;
  String region = MyAssign.region;
  String capital_city = MyAssign.capital_city;
  String continent = null;
  String flagpic = null;
  String curr = null;
  String disp = null;
  JPanel goes_south = new JPanel();
  JPanel goes_center = new JPanel();
  JPanel goes_west = new JPanel();
int ind = 0;  
  String[][]countries=
  	{
  		{"India",			"MISSION TO MARS",			"mom.jpg",		"My"  , "The Mars Orbiter Mission (MOM), also called Mangalyaan "  },
  		{"United States",	"GOES 15 ",			"goes15.jpg",		"4-Mar-10"," GOES 15 (GOES-P) is an American weather satellite "},
    	{"Russia",			"ISS (ZARYA)",			"zarya.jpg",		"20-Nov-98","The International Space Station (ISS) is a joint project of five space agencies:)"},
    	{"China",			"CBERS",			"CBERS4.jpg",			"7-Dec-14",""},
    	{"Japan",	        "OSUMI",			"o.jpg",		"11-Feb-70",""},
    	{"Australia",		"WRESAT",		"w.jpg",		"29-Nov-67",""},
    	{"Canada",			"Alouette 1",		"a.jpg",		"29-Sep-62",""},
    	{"France",			"ASTERIC",		"f.jpg",		"November 26, 1965 ",""},
    	{"Germany",			"SAR-Lupe",		"s.jpg",		"19-Dec-06"},
    	{"Argentina",	    "ARSAT-1",		"ar.jpg",			"16-Oct-14",""},
    	
    };
  HotPick() throws IOException
  {
	setTitle("Satellite");
	boolean play=false;
    setBounds(50,50,1000,500);

    addWindowListener(new WindowAdapter()
    {
      public void windowClosing(WindowEvent e)
      {
    		setVisible(false);
  	  }
    }
    );
  
    for (int i=0 ; i<24 ; i++)
    {
  		if (countries[i][0].equals(selected_country))
  		{


  			disp=countries[i][4];
  			continent = countries[i][3];
    		flagpic = countries[i][2];
    		curr = countries[i][1];
    		break;
  		}
    }


    	Font labelfont = new Font("",1,24);
    	Font f = new Font("",1,30);

    	JLabel stars = new JLabel("****************" + curr + "*****************");
    	JLabel label_curr = new JLabel(" "+curr);
    	JLabel label_welcome = new JLabel("Belongs TO : "+ selected_country);
    	JLabel label_launch = new JLabel("Launch Date : "+ continent);
    	JLabel label_region = new JLabel("REGION: "+ region);
    	JLabel label_capital = new JLabel("CAPITAL: "+ capital_city);
    	JLabel label_disp = new JLabel("Discription: "+ disp); 
    	
    	
   
    	stars.setFont(f);
    	label_welcome.setFont(labelfont);
    	label_curr.setFont(labelfont);
    	label_region.setFont(labelfont);
    	label_capital.setFont(labelfont);
    	label_launch.setFont(labelfont);

    	ImageIcon flagIcon = new ImageIcon(flagpic);
    	JLabel flagLabel = new JLabel(flagIcon);

		goes_west.setLayout(new GridLayout(8,1));
		goes_west.add(label_curr);
		goes_west.add(label_welcome);
		goes_west.add(label_launch);
    	goes_west.add(label_region);
    	goes_west.add(label_capital);
    	goes_west.add(label_disp);
    	goes_west.setBackground(Color.GRAY);
    	goes_south.setBackground(Color.CYAN);



    	goes_center.add(flagLabel);

    	goes_south.add(stars);

    	getContentPane().add(goes_center,BorderLayout.CENTER);
    	getContentPane().add(goes_south,BorderLayout.SOUTH);
    	getContentPane().add(goes_west,BorderLayout.WEST);

    	System.out.println("Country:" + selected_country);
    	System.out.println("Continent:"+ continent);
    	System.out.println("Currency:"+ curr+ "\n");
    	
    	if(countries[0][0].equals(selected_country)&& play==false)
    	    try {
    	        AudioInputStream audioInputStream = AudioSystem.getAudioInputStream(new File("a1.wav"));
    	        Clip clip = AudioSystem.getClip();
    	        clip.open(audioInputStream);
    	        play=true;
    	        clip.start();
    	        
    	    } 
    	catch(Exception ex) {
    	        System.out.println("Error with playing sound.");
    	        ex.printStackTrace();
    	    }

  }
}
