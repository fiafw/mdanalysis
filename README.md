# MD trajectory analysis

# Menggabung files: 
> gmx **trjcat** -f md_1.xtc md_2.xtc -o md_500ns.xtc -settime
# Frame pertama tulis: 0
Frame kedua: 250000

Recentering protein: 
> gmx **trjconv** -s md_1.tpr -f md_500ns.xtc -o md_500ns_center.xtc -center -pbc mol -ur compact
Pilih 1 (Protein)
Kemudian 0 (System)

1. RMSD
   Generating RMSD Backbone: gmx rms -s md_1.tpr -f md_500ns_center_xtc -o rmsd_backbone.xvg -tu ns (Pilih 4, lalu 4)
   Generating RMSD Ligand: gmx rms -s md_1.tpr -f md_500ns_center_xtc -o rmsd_ligand.xvg -tu ns (Pilih 13, lalu 13)

   Plot Probability Distribution Function (PDF): gmx analyze -f rmsd_backbone.xvg -dist rmsd_backbone_pdf.xvg -bw 0.01
   
3. RMSF
    Generating RMSF Backbone: gmx rmsf -s md_1.tpr -f md_500ns_center_xtc -o rmsf_backbone.xvg -res (Pilih 4)
    gmx analyze -f rmsf.xvg -dist rmsf_pdf.xvg -bw 0.1
   
5. RoG
   gmx gyrate -s md.tpr -f md_center.trr -o RoG.xvg 
   gmx analyze -f gyrate.xvg -dist rg_pdf.xvg -bw 0.1
7. SASA
   gmx sasa -s md.tpr -f md_center.trr -o Backbone.xvg -tu ns
   gmx analyze -f sasa.xvg -dist sasa_pdf.xvg -bw 1

9. Hydrogen bond
   gmx hbond -s md_1.tpr -f md_500ns_center.xvg -num hbond.xvg -tu ns

11. Hydrogen bond (alternatif)
    gmx hbond-legacy -s md_1.tpr -f md_500ns_center.xvg -num hbond.xvg -tu ns

    Print PDF hbond:
    awk '!/^[#@]/ {print $1, $2}' hbnum_BZEM.xvg > hbnum_col1.xvg
    gmx analyze -f hbnum_col1.xvg -dist hbdist_BZEM.xvg -bw 1

13. DSSP
    
14. Free Energy Landscape (FEL) based RMSD and RoG
