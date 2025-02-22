#
# This file is part of the GROMACS molecular simulation package.
#
# Copyright (c) 2010,2011,2012,2013,2014,2015,2017,2018,2019, by the GROMACS development team, led by
# Mark Abraham, David van der Spoel, Berk Hess, and Erik Lindahl,
# and including many others, as listed in the AUTHORS file in the
# top-level source directory and at http://www.gromacs.org.
#
# GROMACS is free software; you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public License
# as published by the Free Software Foundation; either version 2.1
# of the License, or (at your option) any later version.
#
# GROMACS is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# Lesser General Public License for more details.
#
# You should have received a copy of the GNU Lesser General Public
# License along with GROMACS; if not, see
# http://www.gnu.org/licenses, or write to the Free Software Foundation,
# Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301  USA.
#
# If you want to redistribute modifications to GROMACS, please
# consider that scientific software is very special. Version
# control is crucial - bugs must be traceable. We will be happy to
# consider code for inclusion in the official distribution, but
# derived work must not be called official GROMACS. Details are found
# in the README & COPYING files - if they are missing, get the
# official version at http://www.gromacs.org.
#
# To help us fund GROMACS development, we humbly ask that you cite
# the research papers on the package. Check out http://www.gromacs.org.

file(GLOB UTILITY_SOURCES *.cpp)
list(REMOVE_ITEM UTILITY_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/cuda_version_information.hip.cpp)
if (GMX_GPU AND NOT GMX_USE_OPENCL)
if(GMX_USE_CUDA)
    gmx_add_libgromacs_sources(cuda_version_information.cu)
elseif(GMX_USE_HIP)
    gmx_add_libgromacs_sources(cuda_version_information.hip.cpp)
endif()
endif()
set(LIBGROMACS_SOURCES ${LIBGROMACS_SOURCES} ${UTILITY_SOURCES} PARENT_SCOPE)

# TODO: (https://redmine.gromacs.org/issues/988) Find a new convention for defining public API.
install(FILES
        basedefinitions.h
        current_function.h
        gmxassert.h
        real.h
        DESTINATION include/gromacs/utility)

if(GMX_INSTALL_LEGACY_API)
  install(FILES
          arrayref.h
          baseversion.h
          enumerationhelpers.h
          programcontext.h
          range.h
          smalloc.h
          unique_cptr.h
          DESTINATION include/gromacs/utility)
endif()

if (BUILD_TESTING)
    add_subdirectory(tests)
endif()
