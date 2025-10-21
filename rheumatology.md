# Rheumatology SBML Model Documentation

## Model Overview
**Model ID:** rheumatology  
**SBML Level:** 2  
**SBML Version:** 4  
**CellDesigner Version:** 4.0  
**Canvas Size:** 1280 x 1280 pixels

This model represents the biological mechanisms involved in rheumatoid arthritis and psoriatic arthritis, based on three research papers from 2024. The model includes inflammatory pathways, complement system activation, kinase signaling, and tissue destruction mechanisms.

---

## Compartments (Anatomical Structures)

| ID | Name | Size | Units |
|----|------|------|-------|
| default | Default | 1 | volume |
| synovial_tissue | Synovial Tissue | 1 | volume |
| joint_space | Joint Space | 1 | volume |
| cartilage | Cartilage | 1 | volume |
| bone | Bone | 1 | volume |
| extracellular_matrix | Extracellular Matrix | 1 | volume |

---

## Species (Biological Molecules/Cells)

### Cytokines and Inflammatory Mediators
| ID | Name | Compartment | Initial Amount | Units |
|----|------|-------------|----------------|-------|
| TNF | Tumor Necrosis Factor | synovial_tissue | 0 | substance |
| IL1 | Interleukin-1 | synovial_tissue | 0 | substance |
| IL6 | Interleukin-6 | synovial_tissue | 0 | substance |
| IL17 | Interleukin-17 | synovial_tissue | 0 | substance |

### Cell Types
| ID | Name | Compartment | Initial Amount | Units |
|----|------|-------------|----------------|-------|
| FLS | Fibroblast-like Synoviocytes | synovial_tissue | 100 | substance |
| macrophage | Macrophages | synovial_tissue | 50 | substance |
| T_cell | T Cells | synovial_tissue | 30 | substance |
| B_cell | B Cells | synovial_tissue | 20 | substance |

### Integrins and Adhesion Molecules
| ID | Name | Compartment | Initial Amount | Units |
|----|------|-------------|----------------|-------|
| integrin_a11b1 | Integrin α11β1 | synovial_tissue | 10 | substance |
| collagen | Collagen | extracellular_matrix | 1000 | substance |

### Complement System
| ID | Name | Compartment | Initial Amount | Units |
|----|------|-------------|----------------|-------|
| C3 | Complement C3 | joint_space | 50 | substance |
| C5 | Complement C5 | joint_space | 30 | substance |
| autoantibody | Autoantibodies | joint_space | 5 | substance |
| bispecific_antibody | Bispecific Antibodies | joint_space | 0 | substance |

### Kinases and Signaling
| ID | Name | Compartment | Initial Amount | Units |
|----|------|-------------|----------------|-------|
| TYK2 | Tyrosine Kinase 2 | synovial_tissue | 20 | substance |
| zasocitinib | Zasocitinib (TAK-279) | synovial_tissue | 0 | substance |

### Tissue Damage Markers
| ID | Name | Compartment | Initial Amount | Units |
|----|------|-------------|----------------|-------|
| tissue_damage | Tissue Damage | joint_space | 0 | substance |
| cartilage_degradation | Cartilage Degradation | cartilage | 0 | substance |
| bone_erosion | Bone Erosion | bone | 0 | substance |

---

## Reactions (Biological Processes)

### 1. TNF Production and Signaling
- **ID:** TNF_production
- **Name:** TNF Production by Macrophages
- **Reversible:** No
- **Reactants:** macrophage (1)
- **Products:** TNF (1), macrophage (1)
- **Kinetic Law:** macrophage × 0.1
- **Description:** Macrophages produce TNF-alpha, a key inflammatory cytokine

### 2. Integrin α11β1 Binding to Collagen
- **ID:** integrin_collagen_binding
- **Name:** Integrin α11β1 Binding to Collagen
- **Reversible:** Yes
- **Reactants:** integrin_a11b1 (1), collagen (1)
- **Products:** integrin_a11b1 (1), collagen (1)
- **Kinetic Law:** (integrin_a11b1 × collagen × 0.01) - (integrin_a11b1 × collagen × 0.001)
- **Description:** Binding interaction between integrin α11β1 and collagen in extracellular matrix

### 3. Complement Activation
- **ID:** complement_activation
- **Name:** Complement Activation by Autoantibodies
- **Reversible:** No
- **Reactants:** autoantibody (1), C3 (1)
- **Products:** C5 (1), tissue_damage (1)
- **Kinetic Law:** autoantibody × C3 × 0.05
- **Description:** Autoantibodies trigger complement cascade leading to tissue damage

### 4. TYK2 Inhibition by Zasocitinib
- **ID:** TYK2_inhibition
- **Name:** TYK2 Inhibition by Zasocitinib
- **Reversible:** Yes
- **Reactants:** TYK2 (1), zasocitinib (1)
- **Products:** TYK2 (1), zasocitinib (1)
- **Kinetic Law:** (TYK2 × zasocitinib × 0.1) - (TYK2 × zasocitinib × 0.01)
- **Description:** Allosteric inhibition of tyrosine kinase 2 by zasocitinib (TAK-279)

### 5. FLS Activation and Joint Destruction
- **ID:** FLS_activation
- **Name:** FLS Activation Leading to Joint Destruction
- **Reversible:** No
- **Reactants:** FLS (1), TNF (1)
- **Products:** FLS (1), cartilage_degradation (1), bone_erosion (1)
- **Kinetic Law:** FLS × TNF × 0.02
- **Description:** TNF-activated fibroblast-like synoviocytes cause cartilage and bone destruction

### 6. Local Complement Inhibition by Bispecific Antibodies
- **ID:** local_complement_inhibition
- **Name:** Local Complement Inhibition
- **Reversible:** No
- **Reactants:** bispecific_antibody (1), C5 (1)
- **Products:** bispecific_antibody (1)
- **Kinetic Law:** bispecific_antibody × C5 × 0.8
- **Description:** Bispecific antibodies provide targeted local complement inhibition

---

## Parameters

| ID | Name | Value | Units | Constant |
|----|------|-------|-------|----------|
| inflammation_score | Total Inflammation Score | 0 | substance | No |

---

## Rules

### Assignment Rule: Total Inflammation Score
- **Variable:** inflammation_score
- **Formula:** TNF + IL1 + IL6 + IL17 + (tissue_damage × 2)
- **Description:** Calculates a composite inflammation score based on cytokine levels and tissue damage

---

## CellDesigner Implementation Notes

### Species Types for CellDesigner:
- **Proteins:** TNF, IL1, IL6, IL17, integrin_a11b1, collagen, C3, C5, autoantibody, bispecific_antibody, TYK2, zasocitinib
- **Complexes:** None defined yet
- **Genes:** None defined yet
- **RNAs:** None defined yet
- **Antisense RNAs:** None defined yet

### Compartment Layout Suggestions:
1. **Synovial Tissue** (center) - Contains most cellular components and cytokines
2. **Joint Space** (top) - Contains complement system and tissue damage markers
3. **Extracellular Matrix** (bottom) - Contains collagen
4. **Cartilage** (left) - Contains cartilage degradation markers
5. **Bone** (right) - Contains bone erosion markers

### Visual Layout Recommendations:
- Place macrophages and FLS in synovial tissue compartment
- Show TNF production from macrophages
- Display integrin-collagen binding interactions
- Illustrate complement cascade activation
- Show TYK2 inhibition by zasocitinib
- Highlight tissue destruction pathways

### Key Pathways to Visualize:
1. **Inflammatory Cascade:** macrophage → TNF → FLS activation → tissue destruction
2. **Integrin-Mediated Adhesion:** integrin_a11b1 ↔ collagen
3. **Complement System:** autoantibody + C3 → C5 + tissue_damage
4. **Therapeutic Inhibition:** zasocitinib → TYK2 inhibition
5. **Local Therapy:** bispecific_antibody → C5 inhibition

---

## Research Paper Sources

This model is based on three 2024 research papers:

1. **Collagen-binding integrin α11β1 contributes to joint destruction in arthritic hTNFtg mice**
   - Key components: integrin_a11b1, collagen, FLS, TNF, joint destruction

2. **Development of bispecific antibodies that drive selective targeted local complement inhibition on rheumatoid arthritis-related antigens**
   - Key components: complement system, autoantibodies, bispecific_antibody, local inhibition

3. **Highly selective tyrosine kinase 2 inhibition with zasocitinib (TAK-279) improves outcomes in patients with active psoriatic arthritis**
   - Key components: TYK2, zasocitinib, kinase inhibition, therapeutic targeting

---

## Manual Editing Instructions for CellDesigner

1. **Import Species:** Add all species listed above with their respective compartments
2. **Set Initial Values:** Use the initial amounts specified in the species table
3. **Create Reactions:** Add each reaction with proper reactants, products, and kinetic laws
4. **Define Compartments:** Create the 6 compartments with appropriate sizes
5. **Add Parameters:** Include the inflammation_score parameter
6. **Set Up Rules:** Implement the assignment rule for inflammation score calculation
7. **Visual Layout:** Arrange components according to the layout suggestions above
8. **Connect Pathways:** Draw arrows showing the biological relationships between components

This documentation provides all the information needed to manually recreate or edit the rheumatology model in CellDesigner.
